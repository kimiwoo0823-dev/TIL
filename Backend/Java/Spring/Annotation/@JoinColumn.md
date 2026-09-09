Entity의 연관 관계에서 외래 키를 매핑하기 위해 사용

FK를 매핑하는 필드의 이름과 해당 필드의 타입(Entity)의 PK로 FK 이름이 지정

ex)
```
@ManyToOne
@JoinColumn(name = "following_id")
private User following;
```
이렇게 짠 코드는 DB에서 `following_id`는 컬럼명으로 들어가고 내부 값을 User의 PK인 id에서 받아옴