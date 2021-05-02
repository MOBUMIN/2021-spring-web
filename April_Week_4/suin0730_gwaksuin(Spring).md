# Study with Me :)

* [9. Deploy to cloud server](#9-deploy-to-cloud-server)
  * [9.1. Why do we need a cloud server?](#91-why-do-we-need-a-cloud-server-)
  * [9.2. Type of cloud server](#92-type-of-cloud-server)
* [10. Create a database on the cloud server](#10-create-a-database-on-the-cloud-server)
  * [10.1. What is H2DB?](#101-what-is-h2db-)
  * [10.2. Why RDS?](#102-why-rds-)
* [11. References](#11-references)

## 9. Deploy to cloud server

### 9.1. Why do we need a cloud server?

앞 절에서 만든 Spring Boot 게시판 서비스를 모두가 사용하게 하려면 어떻게 해야 할까요?
외부에서 제가 만든 서비스에 접근하려면 24시간 작동하는 서버가 필요합니다.
이동욱 저자님은 24시간 서버에 세가지 선택지가 있다고 소개해주셨습니다.

* 집에 24시간 PC 구동시키기
* 호스팅 서비스 이용하기
* 클라우드 서비스 이용하기

클라우드와 호스팅의 가장 큰 차이는 유연성입니다.
호스팅 서비스는 IDC에 물리 서버를 실제로 구축해서 서비스하지만
클라우드 서비스는 서버를 가상화해 사용자의 필요에 따라 실시간으로 확장과 축소가 가능하다는 점에서 이점이 있습니다.

### 9.2. Type of cloud server

[Amazon](https://url.kr/7fem5q)에 따르면,
Elastic Computer Cloud(EC2)는 안전하고 크기 조정이 가능한 컴퓨팅 용량을 클라우드에서 제공하는 웹서비스입니다.
EC2는 개발자가 더 쉽게 클라우드 컴퓨팅 작업을 할 수 있도록 돕습니다.
예를 들어 서버 로그 관리, 모니터링, 하드웨어 교체, 네트워크 관리 등을 지원해줍니다.
제 게시판 앱은 IaaS에 해당하는 AWS EC2에 올라갔지만
사용자가 얼마나 서버를 관리하는지, 얼마나 기술을 제공받는지에 따라 클라우드 컴퓨팅은 몇가지 형태로 나뉩니다.

* Infrastructure as a Service(IaaS)
  * 기업이 준비한 환경에서 개발자들이 장비를 선택할 수 있는 서버
  * 고객은 가상 서버 하위 레벨에 대해서 고려할 필요가 없다.
  * AWS의 EC2처럼 원하는 OS를 깔아 서버로 바로 사용할 수 있다.
* Platform as a Service(PaaS)
  * 클라우드에서 컴파일해서 결과를 가져올 수 있게 하는 형태의 서버
  * 개발자는 node.js, java와 같은 런타임을 깔아놓고 소스코드만 적어서 빌드한다.
  * heroku, google app engine, ibm bluemix 등이 있다.
* Software as a Service(SaaS)
  * 모든 것을 클라우드에서 제공하고 사용자는 별도 설치에 대한 부담이 없다.
  * public cloud에 있는 SW를 웹 브라우저로 불러와 언제 어디서든 사용할 수 있다.
  * 웹 메일, 구글 클라우드, 네이버 클라우드, 드롭박스 등이 있다.

## 10. Create a database on the cloud server

### 10.1. What is H2DB?

깃 정리를 할 때는 자세히 남기지 않았는데 section3에서 JPA를 공부할 때 h2라는 web console을 사용했습니다.
이번 섹션에서는 AWS에서 제공하는 RDS에 대해 알아볼텐데, 그 전에 이 데이터베이스와 RDS의 차이가 뭔지 알아보겠습니다!
데이터베이스는 영구 데이터베이스와 인 메모리 데이터베이스로 나누어 생각할 수 있습니다.

* 영구 데이터베이스: 실제 메모리에 데이터를 유지하므로 서버가 반송되더라도 다시 사용할 수 있음
* 인 메모리 데이터베이스: 데이터는 시스템 메모리에 저장되며 프로그램을 닫으면 데이터가 손실됨

H2DB는 자바 기반의 오픈소스 관계형 데이터베이스 관리 시스템입니다.
서버 모드와 임베디드 모드의 인메모리 DB 기능을 지원하고 브라우저 기반의 콘솔모드를 이용할 수 있으며 용량이 가볍습니다.
또한 표준 SQL 대부분 문법이 지원됩니다.
가볍고 빠르며 IntelliJ와의 호환성도 좋기 때문에 어플리케이션 개발 단계의 테스트 DB로써 많이 사용됩니다.
하지만 램에 데이터를 저장하다보니 웹서버를 재부팅하면 기존 데이터가 사라지고 저용량만 지원한다는 한계가 있습니다.

### 10.2. What is RDS?

만약 직접 데이터베이스를 설치하면 모니터링, 알람, 백업 등을 구성해야 하고 이는 시스템 관리를 복잡하게 만듭니다.
Amazon Web Service에서는 클라우드 기반의 Relational Database Service(RDS)라는 데이터베이스 관리 시스템을 제공합니다.
RDS를 사용하면 개발자는 데이터베이스 설정, 패치 및 백업과 같은 운영 작업을 자동화해 개발 작업에만 집중할 수 있습니다.
만약 데이터베이스에 갑자기 많은 양의 데이터가 쌓여도 RDS를 사용하면 과금으로 손쉽게 용량을 늘릴 수 있습니다!
그러나 RDS는 과금을 해야 한다는 큰 단점이 있기 때문에 EC2 리눅스 위에 직접 DB를 설치하고 서비스하는 옵션도 생각해볼 수 있겠습니다.

[RDS의 한계점](https://url.kr/tfk1l8)

## 11. References

Section 9

* <https://www.comworld.co.kr/news/articleView.html?idxno=49797>
* <https://wnsgml972.github.io/network/2018/08/14/network_cloud-computing/>

Section 10

* <https://developerhive.tistory.com/34>
* <https://dololak.tistory.com/285>
* <https://youngjinmo.github.io/2020/03/h2-database/>
* <https://ko.wikipedia.org/wiki/H2_(DBMS)>
