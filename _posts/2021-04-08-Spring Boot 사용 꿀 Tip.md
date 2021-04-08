<h2>Spring Boot 사용 꿀 Tip</h2>

<h4>DevTools 사용</h4>

수정시 매번 애플리케이션을 재실행하는 것이 귀찮으면 스프링 부트가 제공하는 DevTools를 사용하면 된다.

```markdown
pom.xml → <dependency> 태그 밑에서 ctrl + Space → Edit Starters.. → pom.xml 수정 내용 저장 알림이 뜨면 Save Pom 선택 → Frequently Used에 DevTools가 있으면 체크, 없으면 왼쪽 Available에서 Core를 확장해 DevTools를 추가(내 버전에선 Developer Tools에 Sprnig Boot DevTools로 되어있음)
```

> Spring Boot 버전
>
> Version: 3.9.12.RELEASE
>
> Platform: Eclipse 2019-09 (4.13.0)

<br>

<h4>롬복(Lombok) 사용</h4>

웹어플리케이션에서 사용하는 VO(Value Object) 클래스는 일반적으로 테이블의 Column 이름과 동일한 private 멤버 변수를 가진다. 그리고 이 변수에 접근하기 위해선 Getter/Setter 메소드와 toString()메소드가 필요하다.

이때, 너무 많은 소스코드로 인해 지저분해지기 때문에 롬복(Lombok)을 사용한다. 롬복을 사용하면 자바 파일을 컴파일할 때, 자동으로 생성자, Getter, Setter, toString()과 같은 코드들을 자동으로 추가해준다.

<h5>롬복 라이브러리 등록 방법</h5>

```markdown
pom.xml → <dependency> 태그 밑에서 ctrl + Space → Edit Starters.. → pom.xml 수정 내용 저장 알림이 뜨면 Save Pom 선택 → Available에서 Core를 확장해 Lombok 추가(내 버전에선 Developer Tools에 Lombok으로 되어있음)
```

여기까진 라이브러리를 다운로드한 과정이며, 롬복을 사용하려면 **이클립스(STS) 설치 폴더에 롬복 라이브러리**를 추가해야 한다.

```markdown
https://projectlombok.org/download → lombok.jar 파일 다운로드 → cmd 창 열기 → lombok.jar 파일을 다운로드한 폴더로 이동 후 'java -jar lombok.jar' 입력 → 설치 화면이 뜨면 IDEs 항목에 현재 사용하는 이클립스(또는 STS)의 위치가 출력된다. 실행파일(.exe)의 경로를 확인하고 Intall/Update 클릭 → 설치 금방됨. Quit Installer 클릭 → 이클립스(STS) 설치 폴더에서 lombok.jar가 설치되어 있는지 확인 → 이클립스(STS) 재실행
```

참고로 @Data는 @Getter, @Setter, @RequiredArgsConstructor, @ToString, @EqualsAndHashCode 모두를 포함한다.

<br>

<h4>pom.xml에 오류가 발생했어요</h4>

\<maven-jar-plugin.version>3.1.1</maven-jar-plugin.version\> 설정 추가 후 프로젝트 Refresh

<br>

<h4>스프링과 JPA연동</h4>

maven은 필요한 라이브러리를 특정 문서(pom.xml)에 정의해 놓으면 네트워크를 통해서 라이브러리들을 자동으로 다운받는 특징이 있다. 즉, 생성된 프로젝트에 새로운 의존성을 추가하기 위해선 pom.xml에 \<dependency> 설정을 추가해야 한다.

스프링과 JPA(Java Persistence API)를 연동해서 데이터베이스 작업을 처리할 때

**여기서, JPA란 *하이버네이트(Hibernate)를 비롯한 모든 *ORM(Object Relation Mapping) 프레임워크의 표준이다.**

>*ORM(Object Relational Mapping) : RDB(Relational Database)는 객체지향적(상속, 다형성, 레퍼런스, 오브젝트 등)으로 접근하기 쉽지 않다. ORM은 RDB 테이블을 객체지향적으로 사용하기 위한 기술로, 오브젝트와 RDB 사이에 객체지향적으로 다루기 위한 기술을 의미한다.
>
>*Hibernate : ORM 프레임워크 중 하나로 객체 지향 프로그래밍과 관계형 데이터베이스의 차이로 인해 발생하는 제약사항을 해결하는 해결책이다.

스프링과 JPA를 연동하려면 pom.xml 파일에 하이버네이트 관련 dependency를 추가 한 후, JPA 구현체에 해당하는 Hibernate-entitymanager.jar 파일을 다운로드 후 클래스 패스에 추가해야 한다.

<h5>JPA dependency 추가 방법</h5>

```markdown
https://mvnrepository.com/ → 'jpa' 검색 → Hibernate EntityManager Relocation 선택 → Version 선택(나는 5.4.10 버전을 선택) → Maven에 해당하는 내용 복사 후 dependency에 붙여넣기
```

