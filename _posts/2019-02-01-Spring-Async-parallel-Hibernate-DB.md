---
title: "Spring 에서 @Async 를 이용한 동시다발(parallel)적인 Hibernate DB 작업"
date: 2019-02-01
categories: spring
Tags: spring,async,parallel,hibernate
---

요번 글에서 다루고자 하는 점은 

1. Spring 에서 Api 처리시
2. Asynchronous 한 Hibernate DB 작업을 하고
3. 모든 결과물을 wait 해서 함께 처리한 뒤 내려줘야 하는 상황   

3가지 요소를 다 처리하는 방법에 대한 고민입니다.

메인페이지에 보여주는 여러가지 위젯을 한번에 내려줘야 하는 상황이었는데, 위젯 1개마다 복잡한 쿼리를 날려 DB 데이터를 가져와야 하는 상황이라 순차적으로 수행하면 7초가 넘게 걸리고 있었습니다. 그래도 이 작업들을 동시에 모든 위젯을 불러온 뒤, 한번에 내려 주는 방식으로 처리하고자 위 3가지 요소가 필요해졌습니다.

## 첫번째 시도, `CompletableFuture`

- concurrency 나 future 에 대해 아예 모르신다면 아래 글등을 읽어보시면 좋습니다.

[Java 8: CompletableFuture in action](https://www.nurkiewicz.com/2013/05/java-8-completablefuture-in-action.html)

회사에서 개발중인 소스에는 이미 통합검색등의 기능에서, 사용자가 입력한 글자를 자동완성해서 필요한 데이터들을 찾아올 때 동시에 다양한 도메인에서 자동완성 검색어를 찾기 위해서 `CompletableFuture` 를 사용하고 있습니다. Mybatis 세션의 경우, 별 문제 없이 해당 작업을 `CompletableFuture.runAsync()` 를 이용해서 여러 task 를 돌린 뒤, `CompletableFuture.allOf(futureTaskArray).join()` 을 이용해 동시에 수행하고 나온 결과물을 기다렸다가 받아 올 수 있었습니다.

 동일한 방법을 이번 작업에 사용해 보았지만, 기존의 통합검색이 Mybatis session 을 이용한 것과 달리 위젯부분은 Hibernate 를 사용하고 있었고, hibernate 는 기본적으로 Synchronized-transactional session 이 필요하기에 오류 발생

    Caused by: org.hibernate.HibernateException: Could not obtain transaction-synchronized Session for current thread
    	at org.springframework.orm.hibernate4.SpringSessionContext.currentSession(SpringSessionContext.java:134)
    	at org.hibernate.internal.SessionFactoryImpl.getCurrentSession(SessionFactoryImpl.java:1014)
    	at com.healing.beauty.dao.BaseDao.getCurSession(BaseDao.java:44)

저희 hibernate 는 미리 만들어 둔 BaseDao 라는 부모 Dao 클래스에서 session 을 잡아다 사용하게 되어있는데, 해당 작업시 `SpringSessionContext` 에서는 아래와 같이 synchorize 한 트랜잭션을 가지고 있는지를 체크하게 됩니다.

    if (TransactionSynchronizationManager.isSynchronizationActive()) {
    			Session session = this.sessionFactory.openSession();
    			if (TransactionSynchronizationManager.isCurrentTransactionReadOnly()) {
    				session.setFlushMode(FlushMode.MANUAL);
    			}
    			SessionHolder sessionHolder = new SessionHolder(session);
    			TransactionSynchronizationManager.registerSynchronization(
    					new SpringSessionSynchronization(sessionHolder, this.sessionFactory, true));
    			TransactionSynchronizationManager.bindResource(this.sessionFactory, sessionHolder);
    			sessionHolder.setSynchronizedWithTransaction(true);
    			return session;
    		}
    		else {
    			throw new HibernateException("Could not obtain transaction-synchronized Session for current thread");
    		}

결국 completableFuture 를 쓸 경우, 이 부분을 통과하지 못했습니다.

## 2. `@Async` 어노테이션 사용

Async 를 사용하기 위한 전제조건은 상세한 설명들이 링크에 있으니 참고바랍니다

[How To Do @Async in Spring | Baeldung](https://www.baeldung.com/spring-async)

`@Async` 는 간단히 말하자면, proxy 를 사용하여 비동기처리를 수행하는 방식이기 때문에, 무조건 같은 서비스내 호출이 아닌 다른 서비스 혹은 컨트롤러에서 서비스 호출등으로 프록시를 거치게 해야 합니다. 그래서 기존에 `WidgetService` 내에서 setting 하는 부분을 떼어내서 `WidgetAsyncService` 를 추가해서 메소드를 만들어주고 위에 어노테이션을 붙였습니다.

    
    	@Async
    	public Future<Void> setDataV3(Widget widget, WidgetFilter filter) {
    		logger.info("========== async  STARTTTT ========== " + widget.getTitle());
    			
    /*** 어쩌구 저쩌구 위젯을 가지고 타입별로 Dao 등과 연계해서 필요한 데이터를 불러오는 기능 
    ***/
    
    		logger.info("========== async  ENDDDDDDDDDD ========== " + widget.getTitle());
    		return new AsyncResult<>(null);
    	}

그리고 수행했는데 결과가 똑같.... 또 동기화된 트랜잭션이 없답니다. 

그래도 아까와 달리 이번엔 spring annotation 을 이용한 task executor 를 사용하고 있기에, 여기에 Transaction Propagation 을 적용해보았습니다. (참고로 compleatable future 를 사용할때는 propagation 을 적용해도 변화가 없었다.)

## 3. `@Async` + `@Transactional(Propagation.NOT_SUPPORTED)`

기본적으로 해당 프로젝트의 모든 @Service 에는 @Transactional 이 걸려있습니다. 그렇기에 모든 서비스들이 트랜잭션 내에서 수행되고 있다고 생각하면 됩니다. 그런데 @async 는 기본적으로 `ThreadLocal` 에서 벗어나 수행되면서 Synchronize 한 트랜잭션을 가지지 못하게 되고 그러면서 hibernate 수행이 되지 않았습니다.

그래서 Propagation 룰을 바꿔주기로 했습니다. (propagation 을 모르신다면 아래 멋진 분들의 멋진 글들을 확인)

[spring transaction 전파](http://wonwoo.ml/index.php/post/966)

이 케이스에서는 `Propagation.NEVER` 는 트랜잭션을 전달하지 않기에 위와 같은 오류가 날 것이므로, `NOT_SUPPORTED` 나 `REQUIRED_NEW` 가 필요한데, 어차피 조회만 하는 작업이기에 부모-자식 트랜잭션 상관없기에 `NOT_SUPPORTED` 로 하고 , `readOnly` 도 true 로 주었습니다.

    // 비동기로 처리하는 부분 WidgetAsyncService 내.
    	@Transactional(propagation = Propagation.NOT_SUPPORTED, readOnly = true)
    	@Async
    	public Future<Void> setDataV3(Widget widget, WidgetFilter filter) {
    		logger.info("========== async  STARTTTT ========== " + widget.getTitle());
    		/*** 어쩌구 저쩌구 위젯을 가지고 타입별로 Dao 등과 연계해서 필요한 데이터를 불러오는 기능 
    		***/
    		
    		logger.info("========== async  ENDDDDDDDDDD ========== " + widget.getTitle());
    		return new AsyncResult<>(null);
    	}

이번엔 성공한 코드이기 때문에 비동기를 부르는 동기부분 서비스까지도 추가!

> 비동기 처리가 제대로 돌아가는지 보기 위해선 꼭 로깅이 필요합니다. 동기로 처리하는 부분이 어디까지 수행되고 그 사이에 비동기 처리 부분이 제대로 동기 프로세스와 별개로 수행 되는지 보기 위해 꼭  비동기 메소드 수행 전,후와  비동기 메소드 안에서도 메소드 전체의 시작과 끝에 로깅을 해서 자신의 코드가 의도한 대로 수행 되는지 지켜보겠습니다.

    @Transactional
    public PagingResult<Widget> getWidgetListByV3(WidgetFilter filter) {
    		// dao 에 접속해 먼저 위젯 리스트 불러오기
    		PagingResult<Widget> result = getDao().getListBy(filter);
    		// 비동기 처리의 결과를 담을 array
    		List<Future> futures = new ArrayList<>();
    		logger.info(" ============== 111");
    
    		// 위젯마다 필요한 데이터 세팅
    		for(Widget widget : result.getData()) {
    			logger.info("========== method start ========== " + widget.getTitle());
    			futures.add(widgetAsyncService.setDataV3(widget, filter));
    			logger.info("========== method end ========== " + widget.getTitle());
    		}
    
    
    		logger.info(" ============== 222");
    		// 완료대기
    		for(Future future : futures) {
    			try {
    				// AsyncResult 와 같은 Future object 들은 get 을 이용해 결과 값을 받을때까지 wait 할 수 있다.
    		    // 이 예제에선 void 형태의 리턴값 없는 비동기 메소드였지만, 결과 값이 필요한 경우 비동기메소드의 return 에서 AsyncResult 에 넣어주고
    				// 아래 Future.get() 을 이용해 해당 값을 받아내서 처리할 수 있다.
    				future.get();
    			} catch(ExecutionException e) {
    				e.printStackTrace();
    			} catch(InterruptedException e) {
    				e.printStackTrace();
    			}
    		}
    
    		logger.info(" ============== 333 ");
    		// 위젯 필터링 및 정렬
    		List<Widget> finalList = sortAndFilterWidget(result.getData());
    
    		result.setData(finalList);
    		logger.info(" ============== 444 ");
    		return result;
    	}

만약, 비동기로 수행하는 부분이 결과값도 필요없는 경우라면 (count 를 올리거나, push 를 보내거나 하는 api response 에 필요한 부분이 아니라면) 위에 처럼 get 을 해서 굳이 결과값을 **기다릴 필요 없이 바로 return 해버리면 됩니다.**

자 아무튼 그래서 위처럼 수행하면 예상되는 결과는 아래처럼 로그로 나옵니다.

    //로그 내용이 많아 축약했습니다.
    2019-02-03 02:20:25.234  INFO --- [-nio-8080-exec-5] c.healing.beauty.service.WidgetService   [160] :  ============== 111
    2019-02-03 02:20:25.242  INFO --- [-nio-8080-exec-5] c.healing.beauty.service.WidgetService   [164] : ========== method start ========== 강언TV
    2019-02-03 02:20:25.244  INFO --- [executor-task-19] c.h.beauty.service.WidgetAsyncService    [ 80] : ========== async  STARTTTT ========== 강언TV
    2019-02-03 02:20:25.244  INFO --- [-nio-8080-exec-5] c.healing.beauty.service.WidgetService   [166] : ========== method end ========== 강언TV
    2019-02-03 02:20:25.249  INFO --- [-nio-8080-exec-5] c.healing.beauty.service.WidgetService   [164] : ========== method start ========== 강언's PICK
    2019-02-03 02:20:25.250  INFO --- [-nio-8080-exec-5] c.healing.beauty.service.WidgetService   [166] : ========== method end ========== 강언's PICK
    2019-02-03 02:20:25.251  INFO --- [-nio-8080-exec-5] c.healing.beauty.service.WidgetService   [164] : ========== method start ========== 숨은 보석 이벤트
    2019-02-03 02:20:25.251  INFO --- [-nio-8080-exec-5] c.healing.beauty.service.WidgetService   [166] : ========== method end ========== 숨은 보석 이벤트
    2019-02-03 02:20:25.252  INFO --- [-nio-8080-exec-5] c.healing.beauty.service.WidgetService   [170] :  ============== 222
    2019-02-03 02:20:25.252  INFO --- [executor-task-19] c.h.beauty.service.WidgetAsyncService    [ 80] : ========== async  ENDDDDDDDDDD ========== 강언TV
    2019-02-03 02:20:25.255  INFO --- [ executor-task-7] c.h.beauty.service.WidgetAsyncService    [ 80] : ========== async  STARTTTT ========== 숨은 보석 이벤트
    2019-02-03 02:20:25.255  INFO --- [ executor-task-1] c.h.beauty.service.WidgetAsyncService    [ 80] : ========== async  STARTTTT ========== 강언's PICK
    2019-02-03 02:20:26.552  INFO --- [ executor-task-1] c.h.beauty.service.WidgetAsyncService    [293] : ========== async  ENDDDDDDDDDD ========== 강언's PICK
    2019-02-03 02:20:26.674  INFO --- [ executor-task-7] c.h.beauty.service.WidgetAsyncService    [293] : ========== async  ENDDDDDDDDDD ========== 숨은 보석 이벤트
    2019-02-03 02:20:26.674  INFO --- [-nio-8080-exec-5] c.healing.beauty.service.WidgetService   [182] :  ============== 333 
    2019-02-03 02:20:26.675  INFO --- [-nio-8080-exec-5] c.healing.beauty.service.WidgetService   [187] :  ============== 444

111 부분이 순차적으로 수행되고, 비동기 메소드가 위젯 숫자만큼 실행되는데, 메소드 전후의 로그들이 비동기이기에 바로 순서대로 쭉 실행 되면서 루프가 끝나면 바로 222가 찍힙니다. 그와 동시에 위젯 내용을 불러서 세팅하는 메소드들이 동시 다발적으로 Async 작업을 시작하고 개별적으로 끝나게 됩니다. 

**즉 222를 찍는 부분과 각 위젯내용을 비동기로 불러오는 부분은 서로 병렬하게 진행됩니다.**

**단, 333 이전에 모든 future 작업을 get 해서 처리하기 때문에 333은 모든 async method end 이후에 찍히게 될 것이고** , 그 후 정렬 작업등 뒤에 444 가 찍힌 뒤 모든 작업이 끝나고 결과물을 내려주게 됩니다.

    위 로그를 보면 [executor-task-19] 처럼 Async 에서 사용하는 비동기 태스크의 번호를 볼 수 있습니다. Async 사용 이전에 세팅으로 동시 사용할 수 있는 executor 의 숫자 라던가, 해당 작업이 hibernate 와 DB 작업 이기에 connection pool 개수 등을 너무 작게 세팅해 두었다면 동시에 할 수 있는 작업이 한계가 있어서 로그가 예상과 다르게 찍힐 수 도 있습니다. (실제로 첫번째 시도했을 때 커넥션풀 개수가 5개 였는데 조회된 위젯이 10개였어서 5개 작업만 동시에 수행 되었다.)

## 마무리

위 방법이 가장 좋은 방법이라기보단, 적절하게 성공한 유일한 방법이라서 정리해 보았습니다. 좀더 `Future` 클래스와 Transaction session 에 대한 깊이가 있다면 다른 방법으로도 사용이 가능할지 모르겠습니다.

어쨌든, 동시다발적으로 DB 에 붙어서 필요한 데이터를 조회하고 , 해당 결과를 모아서 다시 api response 로 내려줄 필요가 있다면 이와 비슷한 방식으로 해결할 수 있습니다.

**결과적으로 이전에 7초 8초 정도 걸리던 작업은 1초 만에 해결되었습니다**. 거기에 추가적으로 캐싱까지 걸어서 5분에 1번만 1초정도 걸리고 나머지 요청들은 200ms 정도만에 처리되게 변경하였습니다. 끝!