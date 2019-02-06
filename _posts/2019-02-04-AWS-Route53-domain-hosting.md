---
title: "AWS Route53 으로 기존에 구매한 domain 연결하고 hosting 하기"
date: 2019-02-04
categories: aws
Tags: aws,route53,domian,hosting
---

# 특정 domain 을 이미 가지고 있을 때, 앞으로 AWS 로 호스팅 및 관리하고 싶다면?!

Aws Route53 에 들어가서 hosted zone 추가

- 나의 root 도메인 입력해서 생성

![]({{ site.url }}{{ site.baseurl }}/assets/images/Export-032d9667-fedd-4180-8cc2-e3d28de21c30/Untitled-e891b6f3-c2a1-4975-9c81-4ec7f896b193.png)

자동으로 ns 랑 soa 가 생성된다. name server 의 TTL 을 짧게 줄이고 , 해당 서버 주소들을 복사해 두자.

![](Route53ManagementConsole2019-01-2000-54-58(1)-73ad5528-5b1a-4579-aa97-ace2e4763a72.jpg)
![]({{ site.url }}{{ site.baseurl }}/assets/images/Route53ManagementConsole2019-01-2000-54-58(1)-73ad5528-5b1a-4579-aa97-ace2e4763a72.jpg)

내 도메인을 관리하는 곳으로 가자. TT 나 블루웹이나 뭐 엄청 많은 도메인 제공 사이트들이 있는데, 필자는 .app 도메인을 사려다보니 GoDaddy 라는 곳에서 구매했다. 어떤 사이트던 간에 내 도메인의 DNS 를 관리하는 메뉴가 있을것이니 잘 찾아서 들어가보자. 그러면 그쪽에서 제공하는 네임서버가 아닌 커스텀하게 바꿀 수 가 있는데 이 때, 해당 네임서버를 위에서 복사한 AWS route53의 네임서버를 이용해서 변경한다.

![](2019-01-2000-54-09(1)-7e7b094b-5ee1-4ae3-b735-d0dce6e2c3ce.jpg)
![]({{ site.url }}{{ site.baseurl }}/assets/images/2019-01-2000-54-09(1)-7e7b094b-5ee1-4ae3-b735-d0dce6e2c3ce.jpg)

적용하면 꽤 시간이 흐른뒤에 적용되는데 이 정확한 시점은 나의 부족한 지식으로는 잘 모르겠다. ( email 로 알려주는 곳도 있음) 

## 마무리

어쨌든 필요한 작업은 끝났다. 이제 aws 에서 당신의 도메인을 호스팅하게 되었다. 이는 추후 MX 레코드들을 이용해 도메인 이메일을 관리하거나 (Gsuite 등에서) 할 때도 계속 만나게 될 기능이다.

route53 에서 레코드를 생성해서 그림과 같이 다양한 서브 도메인을 만들고 적용해보자.

![](Untitled-27839e64-63fb-4bc2-b3a2-a480e20da889.png)
![]({{ site.url }}{{ site.baseurl }}/assets/images/Untitled-27839e64-63fb-4bc2-b3a2-a480e20da889.png)