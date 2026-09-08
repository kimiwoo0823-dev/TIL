### record의 등장

DTO를 구현할 때 getter, setter과 같은 데이터 처리를 수행하기 위해 오버라이드된 메서드를 반복해서 작성

이렇게 작성하면 보일러 플레이트 코드가 불필요하게 크게 작성될 수 있음

- 보일러 플레이트 코드
	최소한의 변경으로 여러 곳에서 재사용되고 반복적이고 비슷한 형태를 가진 코드

이는 자바가 가지고 있는 단점으로 작용하는데 lombok이나 IDE의 도움으로 간결하게 만들 수 있으나 근본적인 해결책은 아님

이런 문제들을 해결하기 위해 자바를 업그레이드 하면서 생긴 것이 record

record 목표
- 데이터를 간결하게 표현하기 위한 방법
- 불변 데이터를 모델링하는데 집중
- 데이터 지향 메서드 자동 구현
### record의 특징

불변 객체로 abstract로 선언 불가
암시적으로 final로 선언
setter를 통한 값 변경X

record 내 필드(헤더에 나열된 컴포넌트)는 private final로 정의됨

클래스 상속은 불가하나, 인터페이스로는 구현 가능

멤버 변수는 선언이 불가능 하나 static은 생성 가능
헤더에서 정의한 멤버만을 record에서 관리하기 위함

#### record의 구조

`레코드명(헤더), {바디}`의 구조를 가지는데 헤더에 나열된 필드를 컴포넌트라고 부름

```
public record Album(String author, String title) {}
```
위 예제 코드를 보면 이름은 Album이고 private final 필드를 author, title을 가진 record로 봄

컴파일러는 헤더를 통해 내부 필드를 추론하는데, 이 때 String 타입의 author와 title이 있다는 것을 인식

접근자와 생성자, toString, equals, hashCode를 선언하지 않아도 자동으로 구현됨

#### 컴팩트 생성자

생성자 매개 변수를 받는 부분이 사라진 형태
인스턴스 필드를 초기화하지 않아도 컴팩트 생성자 마지막 부분에 초기화 구문이 자동 삽입
표준 생성자와 달리 컴팩트 생성자 내부에서는 인스턴스 필드에 접근 불가

이런 이유로 컴포넌트로 들어온 값을 불변으로 만들거나 불변식이 만족하는지 등의 작업에 적합

```
public record Album(String author, String title) {

	public Album {
		Objects.requireNonNull(author);
		Objects.requireNonNull(title);
	}
}
```
이렇게 선언한 컴팩트 생성자는 일반 생성자처럼 사용 가능

```
Album lemon = new Album("kenshi yonezu", "lemon")
```
### record는 Entity 말고 DTO로 사용

JPA의 Entity로 쓸 수 있지 않을까 할지 몰라도 불가능함

jpa는 프록시 생성을 위해 인수 생성자, non-final 필드, setter 및 non-final 클래스가 없는 엔티티에 의존
프록시 생성을 위해서는 Entitiy 불변이면 안됨

쿼리 결과 매핑 시 객체를 인스턴스화 할 수 있도록 매개변수가 없는 생성자가 필요
-> record는 매개변수가 없는 생성자를 제공X (모든 필드의 값을 입력 후 생성 가능)

접근자 메서드 getter가 필수 명명 규칙을 따르지 않음
record의 getter는 필드명을 그대로 사용
-> 쿼리 결과 처리 후 수행할 getter, setter에 접근 불가

entity는 표준 자바 클래스로 구현해야 해당 기준을 쉽게 충족 가능

읽어온 정보를 변경하지 않을려면 record 타입의 DTO가 적격