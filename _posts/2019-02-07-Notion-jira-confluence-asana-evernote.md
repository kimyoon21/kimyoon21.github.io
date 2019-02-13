---
title: "Notion 체험기 (jira,confluence,asana,evernote 와 비교)"
date: 2019-02-07
categories: notion
Tags: jira,confluence,evernote,협업툴
---

시작하기 전에, 만약 이 글을 다른 플랫폼(미디엄이나 깃헙등) 에서 보고 계시다면,  ***여기에서 보셔야 확실한 노션맛을 보실 수 있습니다.***
[해당 글 Notion 에서 보기](https://www.notion.so/kimyoon21/Notion-jira-confluence-asana-evernote-ae7bad46ed774c969a63fe34a93bd342)

# Notion 이란?

- 약간은 jira 같고, 약간은 confluence 같다가도, evernote 같기도 한 **메모,태스크관리,포스팅 어플리케이션**
- 깔끔하고 심플한듯 하면서도, **사용자가 필요하고 갈구하는 모든 기능이 다 들어있는 느낌**. 엄청나게 커스터마이징이 가능하다.

> 노션의 장점과 단점을 그동안 사용해본 것들과 비교해서 정리해봤습니다. 지극히 주관적이지만 문서/태스크 관리 서비스 써보신 분들은 공감해주시는 분들도 있을거라 생각합니다.

# 그동안 써왔던 태스크와 문서(wiki)작성 App들의 간지러운 부분들

- Jira(태스크) + confluence(문서) *`화살표를 토글해보세요~`*
    - 장점
        - 커스터마이징이 다양하게 가능하다.
        - 웬만한 Open API 와 기능들이 다 존재한다.
        - 클라우드도 되지만 설치형 서버로 띄울 수 도 있어서, 보안에도 좋고, 사실상 완전히 내 맘대로 활용 가능
        - 여러가지 3rd 파티와 플러그인등이 존재해서 다양한 위젯등을 적용해볼 수 있다.
    - 단점
        - 일단 느리다.. 클라우드에서는 아무래도 빠른 속도와 실시간 반응(1초이내) 를 기대하기 어렵다 ㅜ
        - 컨플루언스는 문서 순서 바꾸기나 뭐 보기 바꾸기등이 너무 힘듬(따로 관리 페이지에 들어가서야 가능한다 또 거기선 문서의 내용을 볼 수 없다)
            - (이 글 쓰는 중에 알게 되었는데, confluence 에서 이제 좌측 페이지들의 순서를 바로 바꿀 수 있게 되었다. 이제서야!?!?!)
        - 너무 방식이 복잡하고 배워야 할 러닝커브가 크다
        - Jira 의 경우, 개발자를 제외한 다른 팀원들이 일단 사용을 꺼리게 되는 디자인과 UX (아마 다들 공감하실듯)
            - 깔끔보단 어수선한 느낌.
            - 스프린트등의 태스크 뷰가 하나로 고정되어 있다.(뷰를 바꾸는건 꽤 커다란 작업임)
            - 뷰안에서 사용자에게 보여줄 필드/숨길 필드등을 세팅하는것도 쉽지 않음
        - confluence 자체가 간편하지 않고, 이미 정해진 에디터에 갇혀 있다고 해야 하나? 표를 쓰는 방식이나 글들을 에디팅 하는 것에도 제약이 많다 (이 부분도 새로운 블로그나 회의록에서는 새 표형식을 적용하기 시작하고 있지만.. 너무 늦었다. 이미 에버노트 등에서는 그런 표 다 쓰고 있음)
        - 태스크문서관리 app계의 SAP 를 쓰는 느낌...(SAP 미안해!! 세계 최고지만 느리고 복잡한거 사실이잖아 !!ㅠㅠ)
- Asana (태스크)
    - 장점
        - 빠르고  디자인이 예쁘다
        - 태스크 관리등에서 custome field 나 property 같은것을 만들고 수정하는 등이 간편하다
        - 타임라인, 캘린더 등을 통해 태스크를 다양한 뷰로 관리 가능
    - 단점
        - 내 이슈만 보기의 부재 : 리포트 형태로만 주어질뿐, 실제 보드형태에선 필터링이 불가 ㅠ
- Evernote (문서)
    - 장점
        - 문서 작성이 빠르고 간편하며, 여러 디바이스에서의 연동이 매끄럽다.
        - 표 형식등이 매우 편리하고, 다양한 형태의 기록물을 편집할 수 있다
    - 단점
        - 마크다운이나 코드 쓰는 기능은 아쉬움. 개발자에게는 큰 걸림돌
        - 에버노트를 복사해서 다른 곳에 붙여넣었을 때, 기존 형식이 깨지는 일이 많음
        - 테스크 관리나 데이터베이스의 느낌은 없음

---

# 이것들을 극복하는 Notion!!!

## 다른 app 들의 장점들을 모조리 흡수했다

- **커스터마이징이 다양하게 가능하다.**
    - 해당 글에서도 볼 수 있듯이 문서에도 온갖 데이터와 위젯을 끼워넣을 수 있다. ***슬래시/ 명령어로 이미지, 코드, 영상부터 다양한 3rd 파티 까지 삽입 가능!***
        
        <video src="/assets/videos/-7f347118-8821-4598-8e36-3d1b2426b42e.mov" width="640" controls preload></video>

- 빠르고  디자인이 예쁘다
    - 어떻게 꾸미느냐에 따라 매우 아름답게 표현 가능. 다양한 디자인을 확인 가능합니다. (아래는 북마크 추가기능)

    [Notion Pages ⚡️](https://notionpages.com/)

- 태스크 관리등에서 custome field 나 property 같은것을 만들고 수정하는 등이 간편하다
    - 따로 설정 페이지 들어가서 하는게 아니라 실제 데이터를 보면서 실시간으로 보드/테이블 등이 바뀌는것을 볼 수 있음
- 타임라인, 캘린더 등을 통해 태스크를 다양한 뷰로 관리 가능
    - 테이블 형식등이 매우 편리하고, 다양한 형태의 기록물을 편집할 수 있다
    - **동일한 태스크(데이터베이스) 를 가지고 Board, table, calendar, gallery 등 다양하게 볼 수 있음**
         <video src="/assets/videos/-00cbea2d-f2c3-4708-96c9-8c288291ecbd.mov" width="640" controls preload></video>

    - 아래는 그런 데이터베이스를 inline 으로 삽입한 예시 (노션으로 뷰 형태를 바꿔가면서 봐보세요~)  <주의:Notion 에서 볼때만 Database 가 보입니다 >

    [inline 으로 생성한DB](/assets/csv/inline-DB-7810f6d3-231c-495b-9ff2-cefd31b7b23c.csv)

- 문서 작성이 빠르고 간편하며 실시간으로 동시에 가능.

![]({{ site.url }}{{ site.baseurl }}/assets/images/NotionDesktop2019-01-0218-22-07(1)-ae0eaf96-7306-4c39-8509-d00a9f8811a2.jpg)

데이터베이스 에서도 현재 동료가 어느 페이지를 보고있는지 까지 알 수 있다

- 페이지 이력도 남겨주기 때문에 복구나 변경여부등도 쉽게 파악 가능 ( 단, 유료 기능임 - 우측상단 updates 에서 확인 가능)

---

## 그 외 신박한 것들 & 써보면서 알게 된 팁

- 코드블럭
    - 간편하고 강력함 (conflence 처럼 힘들게 위젯 불러와서 세팅 다 해주고, 값 집어넣는거 따로 하고 할 필요 없음, copy 도 바로 되고!)

            var x = 1234/32;
            const y = "test".length() ;
            // 이처럼 코드 블럭도 ``` 이걸로 바로 열리는 마크다운 형식도 사용가능하며, 
            /**바로 어떤 코드타입인지 설정해서 편하게 사용 가능하다.**/

- framer , invision 등 삽입
    - 다른 서비스에도 없던것은 아니지만, 매우 간단하게 삽입이 가능. 아래 폰을 클릭해보세요~  <주의:Notion 에서 볼때만 embedded framer 가 보입니다 >

        [https://framer.cloud/TFPWZ](https://framer.cloud/TFPWZ)

- 템플릿
    - 다양한 템플릿으로 입맛에 맞게 만들 수 있다. 컨플루언스등에도 많지만 개인적으론 훨씬 모던하고, 이쁘면서, 실용적인 템플릿이 많음.
    - 노션 상황별 활용 예시로 있는 사이트..(사실 저는 슥 보고 참고하진 않았지만 )
        - [https://www.notion.so/caa92d12e28842f4a19eb4d6853ee6e1](https://www.notion.so/caa92d12e28842f4a19eb4d6853ee6e1)
            - 링크를 활용해 아래같은 북마크도 생성 가능

            [상황별 활용예시 및 노하우](https://www.notion.so/caa92d12e28842f4a19eb4d6853ee6e1)

- 인라인 커멘트와 링크 기능 등
    - 아래그림 처럼 편하게 편집이 가능하고 인라인 커멘트 (하이라이팅)와 날짜 Dec 29, 2018 와 리마인더 등도 삽입이 가능하다.
    - 커멘트를 달면 아래처럼 나오고 (인라인 뿐만 아니라 컨텐츠 자체 커멘트도 비슷) 해당 커멘트를 resolve 해서 해결시킬 수 도 있다.

        ![]({{ site.url }}{{ site.baseurl }}/assets/images/Untitled-6846b6c9-8cc3-4f60-a7ec-6d19ae73ae78.png)

        글 블록한 뒤에 할 수 있는 메뉴들

        ![]({{ site.url }}{{ site.baseurl }}/assets/images/Untitled-7b504571-74f0-4a61-ae5a-5254874b7962.png)

        선택부분만 하이라이트 하고 멘션 달기

- 언제 어디서나 내용이나 페이지를 바로바로 수정 적용
    - 마우스만 쓰고 있을때, 보드 데이터등은 쓰레기통에 드래그로 버리기
    - 바로 외부링크 쉐어 (퍼블릭 링크도 되고, 댓글도 달 수 있고.., 워크스페이스 자체를 공유도 됨)

- import & export 로 기존에 쓰던 서비스에서도 쉽게 가져올 수 있다
    - 페이지는 md, 데이터는 csv 로 익스포트해서 어디든 사용.
    - 임포트도 가능 csv md 등은 당연하고 필자는 jira 와 confluence 임포트도 아래와 같이 성공했다.
        - (지라 → 아사나 → 노션 성공) ,
        - (컨플루언스 → html → 노션도 성공은 했는데 글 제목이 다 왜 숫자id 로 바뀌는지 ㅠ)
        - 더 자세한 방법은 링크 확인

- 한 페이지를 여러 워크스페이스로 보낼 수 도 있다. 워크스페이스간 문서의 이동이 쉽게 가능
- 슬랙에서도 가능한거지만 `⌘ + [ 혹은 ]`  를 이용해서 방금 작업하던 페이지로 뒤돌아갔다가 다시 앞으로 왔다가 이런것들이 가능. 여러 페이지 작업하신 분들이면 슈퍼 필수 기능이다.
- `⌘ +a` 를 1번 누르면 해당 라인 전체를 2번 누르면 모든 문서내용을 선택할 수 있다. 라인(이 작은 단위를 뭐라고 불러야 할지 모르겠지만.. ) 별로 선택이 가능하기에 이런식으로 구분 되어 있어서 더 편함.
- 템플릿 버튼!
    - 특정 작업페이지에는 어떤 픽스된 템플릿에 맞춰서만 추가적인 기록을 가능하게 하고 싶으면, / 눌러서 templiate button 들어간 뒤, 해당 템플릿을 내 입맛에 맞게 맞춰놓고 그 후 사용자들이 그 버튼만 누르면 내가 만든 템플릿이 하래 뚝딱뚝딱 바로 추가된다. 따로 복붙하고 지저분하게 그전꺼 지우고 이럴 필요 없이 깔끔함! (수정권한 없인 못써보기때문에 영상을 첨부했습니다)
        <video src="/assets/videos/-d8056dee-f5cc-42bd-9fb7-60b715a7fe6f.mp4" width="640" controls preload></video>

---

# 그럼에도 불구하고 아쉬운 점?

- 슬랙연동 기능이 있긴 한데, 첫번째로 연동 후 슬랙메시지가 오는 속도가 느리고, 둘째로 너무 하나하나 짜잘한 작업까지 다 슬랙이 오는데 이걸 커스톰처리할 수 없다. 결국 슬랙 연동을 꺼버리게 됨 ㅠ
- ~~앱에서 사용시에 첫페이지가 고정이라 첫페이지를 삭제하면 자꾸 에러가 난다.~~
    - 오류인줄 알았는데 랜딩 페이지가 고정은 아니고 마지막에 있던 페이지로 다시 가게 되는거란다. 첫페이지 지우면 계속 다시 킬때 에러페이지로 랜딩하는데, 이 때 다른 페이지로 간 상태에서 앱을 뒤로가기 누르지 말고 그냥 강제로 꺼버리면 그 후부턴 다른 페이지로 랜딩된다.
- 안드에서 백버튼 누를때 안꺼지고 드로어(왼쪽 목차구조) 열림 좋겠음 (이 부분은 CS 쪽에 요청했더니 이미 다들 비슷한 의견 보내줬다고 고민중이라고 하더라.. 친절한 CS 까지!)
- 너무 쉽게 페이지의 구조나 위치를 바꿔버릴 수 있어서, 그런식의 작업이 일어나고 나면 따로 띠배너같은걸로 방금한 작업을 취소 할 수 있게 해주는 (인스타등에서 댓글 지우거나 하면 나오는) 기능이 있으면 좋겠다. 왜냐하면 때론 그냥 드래그 하다가 실수로 옮겨지곤 해서 내가 방금 실수를 했는지조차 모를 때 도 있음.
- 외부 공유하려고 보니 og-tag 체계가 잘 잡혀있지 않다. 공유하려는 글의 제목이나 이미지가 뜨지 않고, 노션자체 사이트의 타이틀이 뜨거나,, 혹은 아예 뜨지 않는다. 아쉬움 ㅠ  (좀 더 알아보니,, 블로그 글을 모아둔 부모페이지는 공유하면 잘 되는데, 자식페이지인 특정 글을 공유하면 안된다. 왜일까..)
    - .. *아래처럼 보통 기사나 블로그링크는 og 태그 기반으로 제목이나 이미지, 내용등을 미리볼 수 있다*

![]({{ site.url }}{{ site.baseurl }}/assets/images/Untitled-6f7190ee-78cb-4d50-bfde-79a2a5777137.png)

기왕 퍼오는거 훈훈한 자체미담을 제조해보고자 했지만, 교수님 성함만.. 

# 마무리

조금 오버해서 얘기하자면, 처음 slack 을 만났을때의 충격과 비슷합니다.

슬랙도 지금껏 존재하지 않았던 메신저라는 기능을 처음 만든 회사는 아니었지만, 가장 편하고 활용적이면서 깔끔하고 빠르고 확장가능한 서비스를 만들었기에 이렇게 컸다고 생각이 들기에 Notion 도 그러한 길을 (지금처럼 꾸준히 발전한다면) 가지 않을까 생각이 들고 있습니다. (회사의 경우 개인당 월8달러로 쓸 수 있는데, 지라+컨플루언스 둘다 쓰는것보다 당연히 저렴하긴 합니다. 둘다 완전 대체만 가능하다면!)

어쩌다 보니 notion 에반젤리스트 처럼 얘기를 했는데,,,실제로 주식을 살 수 있다면 사두고 싶을 정도입니다 흐흐

만약 이 글을 다른 플랫폼(미디엄이나 깃헙등) 에서 보고 계시다면, 다시한번 여기로 가서 실제 노션에서 사용 가능한 모든 기능을 (모든까진 아니지만 ㅋㅋ) 체험해보시길 바랍니다.

어쨌든 현재 상황과 규모에 잘 맞는 서비스를 잘 선택해서 조금 더 편하고 쉽게 일하는 회사가 되어보길 바랍니다. (일단 우리부터..)

아래 글이 지금까지 본것중 가장 잘 정리되고 실무에 적용한 사례라서 추가.

[Notion 1년간의 사용기](https://velog.io/@godori/Notion-1%EB%85%84%EA%B0%84%EC%9D%98-%EC%82%AC%EC%9A%A9%EA%B8%B0-x7jon062yu)