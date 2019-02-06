---
title: "AWS RDS 에서 mysql 한글 및 이모지 처리 (utf8, utf8mb4)"
date: 2019-02-01
categories: mysql
Tags: aws,rds,mysql,emoji,utf8mb4
---

## 시작하며

 mysql 에서 utf8 은 3바이트로 실제 UTF-8 유니코드에서 4바이트를 쓰는것과 달리, 표현할 수 있는 캐릭터제한이 있었다. 한글 표시는 utf8 만 써도 되지만 이모지등은 utf8mb4 를 사용해야 표현이 가능하다.

    java.sql.SQLException: Incorrect string value: '\xEA\xB9\x80\xEC\x9C\xA4...' for column 'name' at row 1

utf8 조차도 안되어 있을때 한글을 db 에 넣으려거나, utf8 만 되어있느데 이모지를 디비에 넣으려면 위와 비슷한 오류가 당신을 막아설 것이다.

자 그러니 DB를 utf8mb4 로 시원하게 세팅해 보는 법을 AWS RDS 에서 해보도록 합시다.

`추가적으로 기본 timezone 도 한국시각에 맞게 바꿔봅니다`

> 주의 : 이미 연결된 커넥션을 모두 꺼버리자. 서버의 파라미터를 수정하고 재부팅했는데, 기존연결된 커넥션에서 파라미터를 계속 조회하면 제대로 변경이 안되어있을 때가 있다.

## RDS parameter group setting

![]({{ site.url }}{{ site.baseurl }}/assets/images/Export-1c2ceeb8-5651-4ea0-a9ae-5521fd98ac7d/Untitled-d7b68757-4650-4807-929b-bc422be2ad17.png)
- RDS - 데이터베이스 - 구성 에 가면 위처럼 파라미터그룹을 볼 수 있습니다. default 로 되어있다면, 새로 하나 생성한 뒤, 그 파라미터 그룹을 사용할 DB 에 적용하면 됩니다.
- 파라미터 그룹 선택 후 편집에서 char 로 치면 character set 에 대한 설정들이 나옵니다. collation 이라고 치면 collation 설정(정렬을 위한) 이 나옵니다.
    - 여기서 해당 값들을 utf8mb4 와 utf8mb4_unicode_ci 로 맞춰줍니다.
        - utf8mb4_general_ci 로 맞춰줘도 상관없습니다 (한글,영어,일본어 등에선 문제 없음. 불어 등에서 정렬이 좀 다릅니다)
    - 단, character_set_filesystem = binary 값 그대로 둬야 합니다.
    - skip-character-set-client-handshake 는 1 로 해줘서 클라이언트에서 접속할 때 주는 값을 막아버립니다.

        ![]({{ site.url }}{{ site.baseurl }}/assets/images/Export-1c2ceeb8-5651-4ea0-a9ae-5521fd98ac7d/Untitled-a676ea31-ccd3-4950-aaaf-22d4f9fc4023.png)
        

            -- 단, 아래처럼 useUnicode=true  는 클라이언트 접속시 꼭 넣어주셔야 합니다 어머님.
            jdbc:mysql://yourRdsEndpoint.[rds.amazonaws.com:3306/foodin_master?useUnicode=true&serverTimezone=Asia/Seoul](http://rds.amazonaws.com:3306/foodin_master?userUnicode=true&serverTimezone=Asia/Seoul)
            -- mySql5.7 부터는 serverTimezone 값도 꼭 넣어야 합니다. 파라미터 그룹에서 time_zone 검색해서 동일하게 Asia/Seoul 로 세팅해두면 now() 등 사용시 현재 한국 시각으로 표시 됩니다.

다 되었으면, 파라미터 그룹 적용 하고, 꼭 DB 를 재부팅 해주시기 바랍니다. RDS 자체적인 오류인지 파라미터 그룹이 동기화가 잘 안되는 경우가 있습니다. 재부팅 후 아까 구성 부분에 다시 갔을때 동기화 라고 써있어야 적용이 잘 된것입니다.

다시 client 툴 등을 사용해서 해당 DB 에 접속 한 뒤,

`show variables where variable_name like 'c%';`  를 날려줍니다.


![]({{ site.url }}{{ site.baseurl }}/assets/images/Export-1c2ceeb8-5651-4ea0-a9ae-5521fd98ac7d/Untitled-de9b2e5d-f0b0-41f7-9bae-767240254e4d.png)

 이 부분이 저도 해결하지 못한 의문인데, 파라미터그룹에선 다 utf8mb4 로 세팅해주었는데 몇몇 값은 여전히 utf8 로 남아있습니다. collation 도 그렇습니다... 재부팅 아무리 해봐도 동일하네요.

근데 일단 문제는 없습니다.

만약 이 작업 전에 이미 디비 스키마와 테이블등을 만들어 두셨다면, 그 테이블들은 일일히 수작업을 해줘야 합니다. 귀찮다면 아래 링크를 보고 활용해보세요.

[[MYSQL] 테이블, 컬럼 charset 변경](https://m.blog.naver.com/blackfrost/40168167133)

utf8mb4 로 잘 세팅되어있는 테이블에 직접 insert into 를 통해 한글을 입력해봅니다. latin1 등의 기본값이 었을 경우엔 해당 컬럼에 ???? 등이 입력이 되어버립니다. 만약 방금전 세팅이 잘 되었다면 한글이 제대로 들어갈것입니다.

## 참고

[AWS RDS parameter group을 이용해 character-set 변경(utf8), 타임존 변경하기](http://devstory.ibksplatform.com/2017/10/aws-rds-parameter-group-character-set.html)