연관관계 어노테이션을 이용해 엔티티 간의 연관관계를 지정해줄 수 있는데 이 어노테이션의 속성으로 로딩 방식을 지정 가능
# 즉시 로딩(FetchType.EAGER)
유저 엔티티가 있고 팔로우 엔티티가 있을 때 팔로우에서 유저의 정보를 받아오는 어노테이션 @ManyToOne의 fetch 속성을 FetchType.EAGER로 지정하여 유저 엔티티와 팔로우 엔티티의 조회 시점을 동일하게 가능

@ManyToOne, @OneToOne의 fetch 타입의 기본값

# 지연 로딩(FetchType.LAZY)
팔로우 엔티티의 @ManyToOne의 fetch 속성을 FetchType.LAZY로 사용하면 조회 시점을 해당 객체가 사용될때로 늦추기 가능

@OneToMany, @ManyToMany의 fetch 타입의 기본값