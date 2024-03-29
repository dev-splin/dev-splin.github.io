---
title: "Database : Index"
excerpt_separator: <!--more-->
categories:
  - CS(Computer Science)
  - Database
tags:
  - CS(Computer Science)
  - "Database : Index"
toc: true
toc_sticky: true
toc_label: 목차
---

# 인덱스(Index)

`인덱스`는 말 그대로 **책의 맨 처음 또는 맨 마지막에 있는 색인**이라고 할 수 있습니다. 이 비유를 그대로 가져와서 인덱스를 살펴본다면 **데이터는 책의 내용이고 데이터가 저장된 레코드의 주소는 인덱스 목록에 있는 페이지 번호**가 될 것입니다. DBMS도 Database 테이블의 모든 데이터를 검색해서 원하는 결과를 가져 오려면 시간이 오래 걸립니다. 그래서 **칼럼의 값과 해당 레코드가 저장된 주소를 키와 값의 쌍으로 인덱스를 만들어 둔 후, 색인화 되어있는 INDEX 파일을 검색하여 검색 속도를 빠르게 하는 것**입니다.

**DBMS의 인덱스는 항상 정렬된 상태를 유지**하기 때문에 원하는 **값을 탐색하는데는 빠르지만 새로운 값을 추가하거나 삭제, 수정하는 경우에는 쿼리문 실행 속도가 느려집니다.** 결론적으로 DBMS에서 인덱스는 **데이터의 저장 성능을 희생하고 그 대신 데이터의 읽기 속도를 높이는 기능**입니다. `SELECT` 쿼리 문장의 `WHERE` 조건절에 사용되는 칼럼이라고 전부 인덱스로 생성하면 데이터 저장 성능이 떨어지고 인덱스의 크기가 비대해져서 오히려 역효과만 불러올 수 있습니다.





## Index의 원리

Index를 해당 컬럼에 주게 되면 초기 Table 생성시, **FRM, MYD, MYI** 3개의 파일이 만들어집니다.

- **FRM(FoRMat)** : 테이블 구조가 저장되어 있는 파일
- **MYD(MYsql Data)** : 실제 데이터가 있는 파일
- **MYI(MYsql Index)** : INDEX 정보가 들어있는 파일

Index를 사용하지 않는 경우, MYI 파일은 비어져 있습니다. 그러나 **Index를 해당 컬럼에 만들게 되면 해당 컬럼을 따로 인덱싱하여 MYI 파일에 입력**합니다. 이후에 사용자가 **SELECT 쿼리로 Index를 사용하는 쿼리를 사용시 해당 Table을 검색하는 것이 아니라 MYI 파일의 내용을 검색**합니다. 만약, **Index를 사용하지 않은 SELECT 쿼리라면 해당 Table Full scan**하여 모두 검색합니다.

![index](https://user-images.githubusercontent.com/79291114/135026275-003b4117-0552-4e99-89ea-4c565cb2367c.png)

이는 책의 뒷부분에 `찾아보기`와 같은 의미로 정리해둔 단어 중에서 원하는 단어를 찾아서 페이지수를 보고 쉽게 찾을 수 있는 개념과 같습니다다. 만약, 이 `찾아보기`가 없다면 처음부터 끝까지 모든 페이지를 보고 찾아야할 것입니다.





## Index의 목적

인덱스의 목적은 RDBMS의 검색 속도를 높이는데 있습니다. SELECT 쿼리의 WHERE절이나 JOIN을 사용했을 때만 인덱스를 사용하며,  색인화 된 `B+ Tree 구조`로 **SELECT 쿼리의 검색 속도를 빠르게 하는데 목적을 두고 있습니다.**

DELETE, INSERT, UPDATE는 Index 사용시 오히려 느려집니다.



**사용하면 좋은 경우**

- WHERE절에서 자주 사용되는 Column

- 외래키가 사용되는 Column

- JOIN에 자주 사용되는 Column



**사용을 피해야 하는 경우**

- Data 중복도가 높은 Column
- SELECT를 제외한 DML이 자주 일어나는 Column



### Index 사용 시, INSERT, DELETE, UPDATE

#### INSERT

- **index split** : 인덱스의 Block들이 하나에서 두개로 나누어지는 현상
  - 인덱스는 데이터가 순서대로 정렬되어야 합니다. 기존 블록에 여유 공간이 없는 상황에서 그 블록에 새로운 데이터가 입력되어야 할 경우, 오라클이 기존 블록의 내용 중 일부를 새 블록에다가 기록한 후, 기존 블록에 빈 공간을 만들어서 새로운 데이터를 추가하게 됩니다.
- 성능면에서 매우 불리합니다.
  - `Index split`은 새로운 블록을 할당받고 Key를 옮기는 복잡한 작업을 수행합니다. 모든 수행 과정이 Redo에 기록되며, 많은 양의 Redo를 유발합니다.
  - `Index split`이 이루어지는 동안 해당 블록에 대해 키 값이 변경되면 안되므로 DML이 블로킹됩니다.



#### DELETE

테이블에서 데이터가 DELETE될 경우, 지워지고 다른 데이터가 그 공간을 사용할 수 있지만, **index에서는 데이터가 DELETE될 경우, 데이터가 지워지지 않고 사용 안됨 표시만 해둡니다.** 즉, 테이블에 데이터가 1만 건 있는 경우, 인덱스에는 2만 건이 있을 수 있다는 뜻입니다. 때문에 인덱스를 사용해도 수행 속도를 기대하기 힘듭니다.



#### UPDATE

**인덱스에는 UPDATE개념이 없습니다.** 테이블에 UPDATE가 발생할 경우, 인덱스에서는 DELETE가 먼저 발생한 후 새로운 INSERT작업이 발생합니다. **DELETE와 INSERT두 개의 작업이 인덱스에서 동시에 일어나 다른 DML보다 더 큰 부하를 주게 됩니다.**





## Index의 장점

- 키 값을 기초로 하여 **테이블에서 검색과 정렬 속도를 향상**
- 인덱스를 사용하면 테이블 행의 고유성을 강화
- 여러 필드로 이루어진(다중 필드)인덱스를 사용하면 첫 필드 값이 같은 레코드도 구분할 수 있음 (액세스에서 결합 인덱스는 최대 10개의 필드 포함 가능)

**테이블의 기본키는 자동으로 인덱스로 설정**되지만, **필드 중에는 데이터 형식 때문에 인덱스 될 수 없는 필드도 있습니다.**

> 데이터 형식이 OLE 개체, 계산된 형식 또는 첨부 파일인 필드는 인덱싱할 수 없습니다.



예를 들어, `이름`, `나이`, `성별` 세 가지의 필드를 갖고 있는 테이블을 생각해봅시다. `이름`은 온갖 경우의 수가 존재할 것이며 `나이`는 INT 타입을 갖을 것이고, 성별은 남, 녀 두 가지 경우에 대해서만 데이터가 존재할 것임을 쉽게 예측할 수 있습니다. 이 경우 어떤 컬럼에 대해서 인덱스를 생성하는 것이 효율적일까요? 결론부터 말하자면 이름에 대해서만 인덱스를 생성하는게 효율적입니다.

왜 성별이나 나이는 인덱스를 생성하면 비효율적일까요? 10000 레코드에 해당하는 테이블에 대해서 2000 단위로 성별에 인덱스를 생성했다고 가정해보면, 값의 range 가 적은 성별은 인덱스를 읽고 다시 한 번 디스크 I/O 가 발생하기 때문에 그 만큼 비효율적인 것입니다. (특정한 레코드를 찾을 때 인덱스 파일을 불러와야 하는데, 범위가 작기 때문에 인덱스 파일을 여러 번 불러와야 해서 그런 것 같습니다.)





## Index 단점

- 인덱스를 만들면 `.mdb` 파일 크기가 늘어남
- 사용자가 한 페이지를 동시에 수정할 수 있는 병행성이 줄어듬
- 인덱스된 필드에서 데이터를 업데이트하거나 **레코드를 추가 또는 삭제할 때 성능이 떨어짐**
- 인덱스가 데이터베이스 공간을 차지재 추가적인 공간이 필요 (DB의 10% 내외의 공간)
- 인덱스를 생성하는데 시간이 많이 소요될 수 있음
- **데이터 변경 작업이 자주 일어날 경우에 인덱스를 재작성해야 할 필요가 있기에 성능에 영향**

어느 필드를 인덱스해야 하는지 미리 시험해보고 결정하는 것이 좋습니다. 인덱스를 추가하면 쿼리 속도가 1초 정도 빨라지지만, 데이터 행을 추가하는 속도는 2초 정도 느려지게 되어 여러 사용자가 사용하는 경우, 레코드 잠금 문제가 발생할 수 있기 때문입니다.

또, 다른 필드에 대한 인덱스를 만들게 되면 성능이 별로 향상되지 않을 수도 있습니다. 예를 들어, 테이블에 회사 이름 필드와 성 필드가 이미 인덱스된 경우에 우편 번호를 필드로 추가해 인덱스에 포함해도 성능이 거의 향상되지 않습니다. 만드는 쿼리의 종류와 관계 없이 가장 고유한 값을 갖는 필드만 인덱스해야 합니다.





## B+, B-Tree 인덱스 알고리즘

DBMS에서 일반적으로 사용되는 인덱스 알고리즘은 `B+, B-Tree` 알고리즘입니다. `B+, B-Tree` 인덱스는 **칼럼의 값을 변형하지 않고(사실 값의 앞부분만 잘라서 관리합니다.) 원래의 값을 이용해 인덱싱하는 알고리즘입**니다.

[B+-Tree 인덱스 알고리즘에 대한 자세한 설명](https://zorba91.tistory.com/293)



### Hash 인덱스 알고리즘

**칼럼의 값으로 해시 값을 계산해서 인덱싱하는 알고리즘**으로 매우 빠른 검색을 지원합니다. 하지만 값을 변형해서 인덱싱하므로, 특정 문자로 시작하는 값으로 검색을 하는 전방 일치와 같이 **값의 일부만으로 검색하고자 할 때는 해시 인덱스를 사용할 수 없습니다.** 주로 메모리 기반의 Database에서 많이 사용합니다.



#### 왜 Index 를 생성하는데 b+tree 를 사용할까요?

데이터에 접근하는 시간복잡도가 `O(1)`인 `Hash Table` 이 더 효율적일 것 같은데, 왜 Index를 생성하는데 `B-, B+ Tree`를 사용할까요? SELECT 질의의 조건에는 부등호(<>) 연산도 포함이 됩니다. **Hash Table 을 사용하게 된다면 등호(=) 연산이 아닌 부등호 연산의 경우에 문제가 발생**합니다. 동등 연산(=)에 특화된 Hash Table은 Database의 자료구조로 적합하지 않기 때문에 Hash를 사용하지 않고 `B-, B+ Tree`를 사용하는 것입니다.





## 결합 인덱스(Composite Index, == 다중 필드 인덱스)

**두 개 이상의 필드를 한 번에 검색하거나 정렬하는 경우가 자주 있으면 해당 필드의 조합에 대한 인덱스를 만들 수 있습니다.** 예를 들어, 동일한 쿼리에서 공급업체 및 제품 이름 필드에 대해 조건을 설정해야 하는 경우가 자주 발생하면 이 두 필드에 대해 결합 인덱스를 만드는 것이 좋습니다.

결합 인덱스를 사용하여 테이블을 정렬하면 Access에서는 **우선 인덱스에 정의된 첫 번째 필드를 사용하여 테이블을 정렬**합니다. 필드의 순서는 결합 인덱스를 만들 때 설정합니다. 첫 번째 필드에 값이 중복된 레코드가 있으면 인덱스에 정의된 두 번째 필드를 기준으로 중복 레코드가 정렬됩니다.

결합 인덱스에는 필드를 최대 10개까지 포함할 수 있습니다.



### 처리 범위와 인덱스

SQL의 성능은 다음의 세가지 항목에 의해 좌우됩니다.

- 처리 범위의 양
- 랜덤 액세스의 양
- 정렬의 양

그럼 처리 범위가 적다는 뜻은 무엇을 의미할까요?? **처리 범위가 적다는 말은 액세스해야 하는 데이터가 적다는 것을 의미**합니다.

액세스해야 하는 데이터가 적다면 우리는 인덱스를 이용하여 성능을 보장 받을 수 있습니다.



#### 예제

```sql
SELECT 카드번호, 사용액
  FROM 거래내역
 WHERE 거래일자 = '200803'
   AND 사용_구분 = '정상'
```

위의 SQL을 위해 생성할 수 있는 인덱스의 종류는 무엇이 있을까요??

1. `거래일자 인덱스`
2. `사용_구분 인덱스`
3. `거래일자 + 사용_구분 인덱스`
4. `사용_구분 + 거래일자 인덱스`



**거래일자 인덱스**

`거래일자` 인덱스를 이용한다면 처리 범위는 `거래일자` 칼럼에 의해서만 감소하게 됩니다. 따라서, `거래일자` 칼럼의 값이 `200803`인 데이터를 모두 액세스할 것입니다.

액세스한 데이터에 대해 `사용_구분` 칼럼의 값이 `정상`인 데이터만을 결과로 추출하게 됩니다.

결국, **거래일자 칼럼에 의해서만 처리 범위가 감소하게 되며 사용_구분 칼럼에 의해서는 처리 범위가 감소하지 않게 됩니다.**



**사용_구분 인덱스**

`사용_구분` 칼럼으로 인덱스를 생성하면 `사용_구분` 칼럼의값이 `정상`인 모든 데이터를 액세스합니다.

`거래일자` 칼럼의값은 처리 범위를 감소시키기 위한 어떠한 역할도 수행하지않게 됩니다.

결국, **사용_구분 인덱스로 인덱스를 생성하게 되면 사용_구분 칼럼의 값만으로 처리 범위가 감소하게 됩니다.**



**거래일자 + 사용_구분 인덱스**

해당 인덱스는 `거래일자` 칼럼의 값이`200803`인 데이터중 `사용_구분` 칼럼의 값이 `정상`인 데이터만 액세스합니다.

`거래일자+사용_구분` 인덱스를 이용하는 순간 처리 범위는 `거래일자` 칼럼에 의해 감소하게 되며 `거래일자` 칼럼에 의해 감소된 처리 범위에서 `사용_구분` 칼럼의 값이 `정상`인 데이터만을 액세스하게 됩니다.

**거래일자 칼럼과 사용_구분 칼럼에 의해 처리 범위가 감소하기 때문에 앞서 언급한 단일 칼럼 인덱스에 비해 처리 범위가 더 많이 감소하게 되므로 성능은 더욱 향상**될 것입니다.



**사용_구분 + 거래일자 인덱스**

세 번째 인덱스와 마찬가지로 `사용_구분` 칼럼의 값이 `정상`인 데이터 중에 `거래일자` 칼럼의 값이 `200803`인 데이터만을 추출하게 됩니다.

따라서, 해당 인덱스도 `사용_구분` 칼럼과 `거래일자` 칼럼에 의해 동시에 처리 범위가 감소하게 됩니다. 이와 같이 **결합 칼럼 인덱스를 생성한다면 처리 범위를 더 많이 감소**시킬 수 있게 됩니다.

이와 같은 이유에서 **단일 칼럼 인덱스보다는 결합 칼럼 인덱스를 생성하는 것이 해당 SQL에대해 더 빠른 성능을 기대**할 수 있게 합니다.



##### WHERE 절에 사용되는 연산자

그렇다고 WHERE 조건에 존재하는 모든 칼럼으로 순서에 상관없이 결합 칼럼 인덱스를 생성하면 인덱스를 구성하는 모든 칼럼에 의해 처리 범위가 감소하게 되는 것은 아닙니다. 먼저, 이를 이해하려면 WHERE 절에 사용하는 연산자의 종류를 이해해야 합니다.

- **점 조건** : IN, = 연산자를 이용한 조건을 의미하며 해당 연산자는하나의 점만을 의미하게 됩니다.
- **선분 조건** : LIKE, BETWEEN, <, > 등과 같이 점 조건을 제외한 연산자를 사용한 조건을 의미합니다. 선분 조건은 하나의 점만을 의미하는 것이 아니면 해당 조건을 만족하는 모든 실수를 의미하게 됩니다.

**이와 같이 WHERE 절에 사용하는 조건은 점 조건과 선분 조건으로 구분**됩니다. **조건에 사용된 연산자에 의해 액세스해야 하는 처리 범위의 차이가 발생**합니다.

- **점 조건 + 점 조건** : 두 조건에 의해 처리 범위 감소
- **점 조건 + 선분 조건** : 두 조건에 의해 처리 범위 감소
- **선분 조건 + 선분 조건** : 앞의 선분 조건에 의해 처리 범위 감소
- **선분 조건 + 점 조건** : 앞의 선분 조건에 의해서만 처리 범위 감소



위 내용은 간단하지만 매우 중요한 의미를 내포하고 있습니다. 예를 들어 확인해 보겠습니다.

```sql
SELECT 카드번호, 사용액
  FROM 거래내역
 WHERE 거래일자 > '200803'
   AND 사용_구분 = '정상'
```

위와 같은 SQL을 수행한다면 앞서 언급한대로 `거래일자 조건은 선분 조건`이며 `사용_구분 조건은 점 조건`이 됩니다. 따라서, 해당 SQL에 대해 최적의 인덱스를 이용하고자 한다면 `사용_구분+거래일자 인덱스`를 생성해야 합니다.

**이와 같이 인덱스를 생성한다면 점 조건+선분 조건으로 구성되므로 두 개의 조건에 의해 처리 범위가 감소**하게 됩니다.

인덱스에서 가장 중요한 요소는 처리 범위를 최소화 시킬 수 있어야 한다는 것입니다.

그 중심에는 결합 인덱스가 존재한다는 것을 명심해야 합니다. 또한, **결합 인덱스를 생성하고자 한다면 점 조건과 선분 조건의 순서에 의해 처리 범위가 변한다는 것에 주의**해야 합니다.



### 결합 인덱스를 구성하는 컬럼의 순서

인덱스를 이용하여 성능 향상의 효과를 기대할 수 있으려면 먼저 해당 인덱스를 이용하여 처리 범위를 최대한 감소시켜야 합니다. 성능을 향상시키기 위해서는 결합 인덱스를 구성하는 컬럼은 반드시 다음의 순서에 맞도록 생성해야 합니다.

- **1순위** : 컬럼이 사용한 연산자에 의한 인덱스 컬럼 선정
- **2순위** : 랜덤 액세스를 고려한 인덱스 컬럼 선정
- **3순위** : 정렬 제거를 위한 인덱스 컬럼 선정
- **4순위** : 단일 컬럼의 분포도를 고려한 인덱스 컬럼 선정

**참고 링크**

[결합 인덱스를 생성하는 우선 순위](http://www.gurubee.net/lecture/2229)





### Primary Index vs Secondary Index

`클러스터(Cluster)`란 **여러 개를 하나로 묶는다는 의미로 주로 사용**되는데, `클러스터드 인덱스`도 크게 다르지 않습니다. `인덱스`에서 `클러스터드`는 비슷한 것들을 묶어서 저장하는 형태로 구현되는데, 이는 주로 비슷한 값들을 동시에 조회하는 경우가 많다는 점에서 착안된 것입니다. 여기서 비슷한 값들은 물리적으로 인접한 장소에 저장되어 있는 데이터들을 말합니다.

`클러스터드 인덱스`는 테이블의 `프라이머리 키`에 대해서만 적용되는 내용입니다. 즉, **프라이머리 키 값이 비슷한 레코드끼리 묶어서 저장하는 것을 클러스터드 인덱스라고 표현**합니다.

`클러스터드 인덱스`에서는 `프라이머리 키` 값에 의해 레코드의 저장 위치가 결정(프라이머리 키로 정렬되기 때문)되며 `프라이머리 키` 값이 변경되면 그 레코드의 물리적인 저장 위치(테이블에서 레코드의 위치를 말함) 또한 변경되어야 합니다. 때문에 테이블에서 가장 효율적인 `프라이머리 키`를 `클러스터드 인덱스`로 사용해야 합니다.

**클러스터드 인덱스는 테이블 당 한 개만 생성**할 수 있습니다. `프라이머리 키`에 대해서만 적용되기 때문입니다, 이에 반해 **non 클러스터드 인덱스는 테이블 당 여러 개를 생성**할 수 있습니다.

> **Primary Index** : 인덱스가 컬럼과 직접적으로 연결되어 위치를 결정
>
> **Secondary Index** : 보조 인덱스. 컬럼의 위치가 어디인지 알려주는 역할



---

참고 : 

[https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/Database](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/Database)

[https://github.com/WooVictory/Ready-For-Tech-Interview](https://github.com/WooVictory/Ready-For-Tech-Interview)

[https://gyoogle.dev/blog/](https://gyoogle.dev/blog/)

[https://github.com/WeareSoft/tech-interview/blob/master/contents/db.md](https://github.com/WeareSoft/tech-interview/blob/master/contents/db.md)

[https://support.microsoft.com/ko-kr/office/%EC%9D%B8%EB%8D%B1%EC%8A%A4%EB%A5%BC-%EB%A7%8C%EB%93%A4%EA%B3%A0-%EC%82%AC%EC%9A%A9%ED%95%98%EC%97%AC-%EB%8A%A5%EB%A5%A0-%ED%96%A5%EC%83%81-0a8e2aa6-735c-4c3a-9dda-38c6c4f1a0ce](https://support.microsoft.com/ko-kr/office/%EC%9D%B8%EB%8D%B1%EC%8A%A4%EB%A5%BC-%EB%A7%8C%EB%93%A4%EA%B3%A0-%EC%82%AC%EC%9A%A9%ED%95%98%EC%97%AC-%EB%8A%A5%EB%A5%A0-%ED%96%A5%EC%83%81-0a8e2aa6-735c-4c3a-9dda-38c6c4f1a0ce)

[https://kdata.or.kr/info/info_04_view.html?field=&keyword=&type=techreport&page=14&dbnum=185103&mode=detail&type=techreport](https://kdata.or.kr/info/info_04_view.html?field=&keyword=&type=techreport&page=14&dbnum=185103&mode=detail&type=techreport)

[http://www.gurubee.net/lecture/2228](http://www.gurubee.net/lecture/2228)

