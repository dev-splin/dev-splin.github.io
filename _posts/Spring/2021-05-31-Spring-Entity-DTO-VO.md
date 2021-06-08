---
title: "Spring : Entity, VO, DTO의 차이"
excerpt_separator: <!--more-->
categories:
  - Spring
tags:
  - Spring
  - "Spring : Entity"
  - "Spring : VO"
  - "Spring : DTO"
toc: true
toc_sticky: true
toc_label: 목차
---

# Entity, VO, DTO의 차이

`Entity, VO, DTO` 클래스는 사람마다 사용방법이 조금씩 다릅니다. 대부분은 `VO(Value Object)`와 `DTO(Data Transfer Object)`를 사용방법이 같다고 생각할 것입니다. 실제로도 비슷하며, 이를 정확히 구분 지어서 사용하는 사람은 많지 않습니다. 하지만 위 3가지의 클래스들을 정확히 구분지어서 사용하는 방법을 알면 클래스를 구분지어야 하는 **기준점**이 생깁니다.



## Entity

**Entity 클래스는 DB의 테이블내에 존재하는 컬럼만을 속성(필드)으로 가지는 클래스**를 말합니다.  `id(PK)`를 통해 구분됩니다.

> RDB(Relational DataBase, 관계형 데이터베이스)에서의 Entity(개체)란, 현실세계에서의 개체를 표현하기 위한 유형, 무형의 실체로써, Entity를 표현하기 위해서 테이블을 생성합니다.

- Entity 클래스는 상속을 받거나 구현체여서는 안됩니다.
- 테이블내에 존재하지 않는 컬럼을 가져서도 안됩니다.
- `Setter`를 사용하면 안됩니다.
  - `Setter`를 무분별하게 사용하면 객체(Entity)의 값을 변경할 수 있으므로 객체의 일관성을 보장할 수 없습니다.
  - 객체의 생성자에 값들을 넣어줌으로써 `Setter` 사용을 줄일 수 있습니다.
    - **기본 생성자 접근 제한자를 protected로 변경하면 new Member() 사용을 제한해 Entity의 일관성을 더 유지**할 수 있습니다.
- 최대한 외부에서 `Getter`를 사용하지 않도록 해당 클래스 안에서 필요한 로직 method을 구현 해야합니다.
  - `Domain Logic`만 가지며 `Presentation Logic`을 가지고 있어서는 안됩니다. (구현 method는 주로 Service Layer에서 사용합니다.)



예를 들어 `User 테이블`이 `id, password, name`을 가지고 있다면, `Entity`클래스의 속성도 `id, password, name`만 가져야합니다.

```java
@Entity
class UserEntity {
	private Long id;
	private String password;
	private String name;
    
    protected UserEntity(){}
    
    public UserEntity(Long id, String password, String name) {
        this.id = id;
        this.password = password;
        this.name = name;
    }
    
    // Getter 생략
}
```

JPA를 사용하게 될 경우 엔티티 클래스에는`@Entity` 어노테이션을 명시해야 합니다. **@Entity는 엔티티 클래스임을 지정하며, 테이블과 1:1로 매핑**됩니다.



## VO(Value Object)

**VO(Value Object)는 말 그대로 값 객체라는 의미**를 가지고 있습니다. 

- VO의 핵심은 **equals()와 hashcode()** 를 오버라이딩 하는 것입니다. 
  - VO **내부에 선언된 속성(필드)의 모든 값들이 VO 객체마다 값이 같아야, 똑같은 객체라고 판별**합니다.
- `immutable(불변성)`을 가집니다.
  - `Setter`를 가질 수 없습니다.
  - DB에서 읽은 값을 VO에 담을 경우, 이 VO의 값이 DB 데이터 원본으로써 신뢰할 수 있습니다.
  - `read only`의 속성을 가진다고 볼 수 있습니다.
- 비즈니스 로직을 가질 수 있습니다.
- 테이블 내에 있는 속성 외에 추가적인 속성을 가질 수 있습니다.
- 여러 테이블(A, B, C)에 대한 공통 속성을 모아서 만든 BaseVO 클래스를 상속받아서 사용할 수 도있습니다.



예를 들어 고객이 구매한 제품을 나타내는 VO가 있으면, `equals()`와 `hashcode()`를 오버라이딩 했기 때문에 해당 VO의 모든 속성 값이 같아야 같은 객체가 됩니다. 즉 어떤 **한 고객이 같은 시간에 같은 물품을 같은 개수만큼 구매한 경우는 하나 밖에 없기 때문에 신뢰할 수 있는 것**입니다.

또, 총 구매 금액을 구하는 **비즈니스 로직들을 추가해 유용하게 활용**할 수도 있습니다.

```java
public class BuyItemVO {
    private Long userId;
	private String itemCode;
	private int price;
	private int count;
	private Date buyDate;
	
    // Getter 생략
    
    ...   
	
	// 총 구매 금액을 구하는 비즈니스 로직
	public int getTotalPrice() {
		return price * count;
	}
	
	@Override
	public int hashCode() {
		// 내용 생략
        ...
	}
    
	@Override
	public boolean equals(Object obj) {
        // 내용 생략
        ...
	}
}
```



## DTO(Data Transfer Object)

**DTO(Data Transfer Object)는 데이터 전송(이동) 객체라는 의미를 가지고 있습니다.** 레이어 간에 데이터를 전달하는 객체 입니다. 여기서 레이어는 `User - Controller - Service - DAO(Repository)` 에서 각 단계를 말합니다.

- `mutable(가변성)`을 가지기 때문에 값을 유연하게 변경할 수 있습니다.
- `Getter`와 `Setter`를 가질 수 있습니다. 
- 별도의 비즈니스 로직을 가지지는 않습니다.
  - 정렬, 직렬화 등 데이터 표현을 위한 기능은 가질 수 있습니다.
- DTO는 주로 비동기 처리를 할 때 사용합니다.
  -  비동기 처리에서도 JSON 데이터 타입으로 변환해야하는 경우, Spring에서 Jackson 라이브러리를 제공하는데, Jackson은 ObjectMapper를 사용해서 별다른 처리 없이도 객체를 JSON 타입으로 변환시켜 줍니다.



예를 들어 위의 BuyItemVO 클래스에 있는 모든 속성을 JSON 형식으로 반환해야 하는 경우 DTO를 따로 만들어 줄 필요는 없지만, 만약에 고객이 구매한 아이템코드와 개수만 JSON 형식으로 파싱하여 보내줘야하는 경우, **데이터 가공 처리를 위해서 DTO를 만들어주는 것**입니다.

```java
public class BuyItemDTO {
    private Long userId;
	private String itemCode;
	private int count;
	
    // Setter, Getter 생략
    
    ...   
}
```





## 정리



### Entity와 DTO

Entity는 각 테이블 컬럼이 모두 선언되어 있어서 DTO를 대신해서 쓸 수 있지 않을까 고민하는 경우가 있을 수 있습니다. 

하지만 **View에서 요청하는 값은 언제나 변경되는 값이고, 그 때마다 Entity를 변경하면 영속성 모델을 구현한 Entity가 순수성을 잃어버릴 수 있어 권장하지 않습니다.**

전체 Layer 중 값이 바뀔 수 있는`User - Controller- Service` 까지는 `DTO`를 사용하고, 그 이후 `Repository`에서 `MapStruct, ModelMapper` 등의 유틸 라이브러리를 사용해 값을 `Entity`로 옮겨 활용하는 식으로 구현하는 것을 권장합니다. 즉 **View Layer와 DB Layer의 역할 분리는 철저하게 하는 것이 좋습니다.**



***하지만, 여러 테이블을 조인해서 데이터를 가져와야 할 때는 어떻게 해야할까요??*** 

*Repository에서는 Entity를 사용하는 게 좋다고 했으니 Etity를 사용해야 한다고 생각할 수 있습니다. 하지만 **Entity는 도메인만** 나타내야 하고 **여러 테이블을 조인하는 과정에서 집계 함수등을 이용해 데이터가 도메인 범위를 벗어날 수 있기 때문에 DTO를 사용하는 게 좋다**고 생각합니다.*



>**영속성 모델** : 데이터를 생성한 프로그램이 종료되더라도 사라지지 않는 데이터의 특성을 말합니다. 영속성을 갖지 않는 데이터는 단지 메모리에서만 존재하기 때문에 프로그램을 종료하면 모두 잃어버리게 됩니다.

[MapStruct의 사용법 및 ModelMapper와의 비교 포스트 링크](https://dev-splin.github.io/spring/Spring-ModelMapper,MapStruct/)



### DTO와 VO

|            | DTO(Data Transfer Object)                     | VO(Value Object)                                |
| :--------: | --------------------------------------------- | ----------------------------------------------- |
|   **값**   | mutable (가변성)                              | immutable(불변성, read only)                    |
|  **전송**  | 레이어 사이의 전송에서 사용 가능              | 레이어 사이의 전송에서 사용 가능                |
|  **식별**  | 내부의 속성(필드)값이 같아도 다른 객체로 식별 | 내부의 속성(필드)값들이 같다면 같은 객체로 식별 |
| **메서드** | Getter, Setter 이외의 기능을 가지지않음       | Getter와 특정한 비즈니스 로직을 가질 수 있음    |



> 주관적인 저의 생각은 이름과 맞게 전송에서는 DTO를 객체에 변하지 않는 신뢰성을 요구할 때 VO를 사용하는 게 좋아보입니다.
>
> 하지만 만약, DTO와 VO클래스를 사용하고 있는데, VO 클래스에 있는 모든 속성을 JSON 형식으로 반환해야 하는 경우 DTO를 따로 만들어 줄 필요는 없지만, 만약 필요한 속성들만 추려서 JSON 형식으로 파싱하여 보내줘야하는 경우에 **데이터 가공 처리를 위해서 DTO를 만들어주는 것**이 좋아보입니다.



---

참고 : [https://webdevtechblog.com/entity-vo-dto-666bc72614bb](https://webdevtechblog.com/entity-vo-dto-666bc72614bb)

[https://velog.io/@gillog/Entity-DTO-VO-%EB%B0%94%EB%A1%9C-%EC%95%8C%EA%B8%B0](https://velog.io/@gillog/Entity-DTO-VO-%EB%B0%94%EB%A1%9C-%EC%95%8C%EA%B8%B0)

[https://jordy-torvalds.tistory.com/81](https://jordy-torvalds.tistory.com/81)

[https://parkadd.tistory.com/53](https://parkadd.tistory.com/53)
