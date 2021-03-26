<h3>스프링 부트(Spring boot)란?</h3>

스프링 프레임워크의 서브 프로젝트로 만들어진, 스프링과 부트의 합성어이다. 스프링(Spring)은 오프소스 프레임워크다. 부트(Boot)는 사용 가능한 상태로 만드는 것으로 즉, 스프링 부트는 **스프링 프레임워크를 사용 가능한 상태로 만들어주는 도구**이다.

<br>

<h3>프레임워크(Framework)란?</h3>

사전적 의미는 '뼈대' 혹은 '구조'로 애플리케이션의 아키텍처에 해당하는 골격 코드이다.

더 구체적으로 설명하자면  **목적에 따라 효율적으로 구조를 짜놓는 개발방식**으로 **특정 운영 체제를 위한 응용 프로그램 표준 구조를 구현하는 클래스와 라이브러리 모임**이라 한다.

> [https://jokergt.tistory.com/89](https://jokergt.tistory.com/89), [https://www.castingn.com/sourcing/kkultip_detail/110](https://www.castingn.com/sourcing/kkultip_detail/110)

<br>

<h3>계층화 아키텍쳐(Layered Architecture)</h3>

시스템을 여러 계층(Layer)으로 나눈 것으로, 아키텍쳐의 컴포넌트들은 각각 애플리케이션의 특정 역할을 수행하도록 나누어져 있다.

주로 3가지 계층인 Presentation, Business, Persistence 로 이루어져 있다.

<br>

<h4> 계층을 분리하는 이유</h4>

한 곳에서 모든 작업이 한꺼번에 이루어진다면

1. 코드의 복잡성 증가
2. 유지보수의 어려움과 유연성 부족
3. 중복 코드의 증가
4. 낮은 확장성

등의 문제가 발생하기 때문이다.

<br>

<h4>1. Presentation Layer (View)</h4>

유저와 브라우저간의 상호작용 로직을 수행하는 계층으로, 사용자의 데이터를 입력받고 요청된 데이터를 출력해 보이는 계층이다.

즉, 외부로부터 애플리케이션에 대한 요구를 받아들이는 동시에 처리 결과를 사용자에게 보여주는 부분으로 Controller(컨트롤러)의 구성 요소와 상호작용한다.

**요약 : 표현과 이벤트 처리 및 데이터 포맷 책임**

<br>

아래는 Presentation 계층에서 가장 많이 사용하는 자바 기반의 오픈소스 프레임워크이다.

<h5>스트러츠</h5>

스트러츠(Struts)는 MVC(Model View Controller) 아키텍처를 제공하는 프레임워크이다.

<h5>스프링(MVC)</h5>

MVC 아키텍처를 제공하지만 스트러츠와 다르게 독립적으로 존재하지 않고 스프링 프레임워크에 포함되어 있다.

<br>

<h4>2. Control Layer (Controller)</h4>

사용자로부터 요청을 받고 응답을 처리하는 계층으로 하위 계층에서 발생하는 Exception이나 Error에 대한 처리를 맡으며, 최종 프레젠테이션 계층에 표현 해야할 도메인 모델을 엮는 기능을 수행한다.

즉, 사용자로부터 전달 받은 데이터의 유효성 검증을 처리하고, 전체 시스템의 설정상태를 유지한다. 요청에 해당한느 비즈니스 로직을 결정하는 역할을 수행한다. 그리고 사용자의 요청을 검증하여 로직에 요청을 전달하고 로직에서 반환된 응답을 적절한 뷰로 연결해준다.

**요약 : 구성 요소간의 처리 흐름을 제어, 데이터 조작 의뢰, 데이터 변환 및 연산, Exception과 Error처리**

<br>

<h4>3. Business Layer (Service)</h4>

애플리케이션의 비즈니스 로직 처리와 비즈니스와 관련된 도메인 모델의 적합성을 검증하고, *트랜잭션을 처리한다. Contol 계층과 Persistence 계층을 연결하는 역할로서 두 계층이 직접 통신하지 않게 하여 애플리케이션의 유연성을 증가시킨다.

**\* 트랜잭션(Transaction)이란? 데이터베이스의 상태를 변화시키기 위해 수행하는 작업의 단위이다.**

> 트랙잭션 설명 참조 사이트 [https://mommoo.tistory.com/62](https://mommoo.tistory.com/62)

> 모델계층 : Business 계층 + Persistence 계층 = Model 계층으로, Model 계층은 애플리케이션에 필요한 지속성 있는 데이터를 조작하며, 데이터베이스 및 파일의 데이터를 조작하는 기능을 수행한다. 또한, 인터페이스를 통해 데이터의 직접적 조작(입력, 수정, 삭제, 조회)을 담당한다.

**요약 : Control 계층과 Persistence 계층을 연결하는 역할, Transaction 처리**

<br>

아래는 Business 계층에서 가장 많이 사용하는 자바 기반의 오픈소스 프레임워크이다.

**스프링(IoC, AOP)**

스프링의 IoC와 AOP모듈을 이용하여 스프링 컨테이너에서 동작하는 비즈니스 컴포넌트를 개발한다.

<br>

<h4>4. Persistence Layer (DAO)</h4>

영구 데이터를 빼내어 객체화 시키며, 영구 저장소에 데이터를 저장, 수정, 삭제하는 계층이다.

**요약 : 데이터베이스나 파일에 접근하여 데이터를 CRUD 하는 계층**

<br>

아래는 Persistence 계층에서 가장 많이 사용하는 자바 기반의 오픈소스 프레임워크이다.

**JPA(Java Persistent API)**

JPA는 하이버네이트(Hibernate)를 비롯한 모든 *ORM(Object Relation Mapping) 프레임워크의 표준이다.

> \* ORM 설명 참조 [https://gmlwjd9405.github.io/2019/02/01/orm.html](https://gmlwjd9405.github.io/2019/02/01/orm.html)

**MyBatis**

마이바티스는 XML 파일에 작성한 SQL을 자바 객체(VO)와 매핑해주는 데이터 매퍼 프레임워크다.

<br>

<h4>5. Domain Model Layer (VO, DTO)</h4>

관계형 데이터베이스의 엔티티와 비슷한 개념을 가지는 것으로 **데이터 객체**를 의미한다.

<br>

<h3>계층간 개발 순서</h3>

보통은 Domain Model -> Persistence -> Service -> Controller -> View 순으로 작업을 진행한다. 이는 특성 상 데이터에 먼저 접근하는 것이 바르기 때문이다.

> 참고 블로그
>
> 1. [https://blog.daum.net/question0921/797](https://blog.daum.net/question0921/797)
> 2. [https://walbatrossw.github.io/etc/2018/02/26/etc-layered-architecture.html#5-domain-model-layer--vo-dto](https://walbatrossw.github.io/etc/2018/02/26/etc-layered-architecture.html#5-domain-model-layer--vo-dto)