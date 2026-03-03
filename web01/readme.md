## 개념적 설계

### 1. Tour & Review System Use Case (GitHub 호환용)

```mermaid
flowchart LR

%% Actors
Guest["비회원"]
Member["회원"]
Admin["관리자"]

%% System Boundary
subgraph "Tour & Review System"

A["관광지 목록 조회"]
B["카테고리별 관광지 조회"]
C["관광지 상세 조회"]

D["리뷰 작성"]
E["리뷰 수정"]
F["리뷰 삭제"]

G["관광지 등록"]
H["관광지 수정"]
I["관광지 삭제"]
J["리뷰 삭제(관리)"]

K["리뷰 목록 조회"]
L["평균 별점 계산"]

M["별점 입력"]
N["리뷰 내용 작성"]

O["작성자 본인 확인"]

end

%% Associations
Guest --> A
Guest --> B
Guest --> C

Member --> A
Member --> C
Member --> D
Member --> E
Member --> F

Admin --> G
Admin --> H
Admin --> I
Admin --> J

%% include 관계 (점선)
C -. "<<include>>" .-> K
C -. "<<include>>" .-> L

D -. "<<include>>" .-> M
D -. "<<include>>" .-> N

E -. "<<include>>" .-> O
F -. "<<include>>" .-> O
```

<br>

### 2. Tour Management Use Case (GitHub 호환용)

```mermaid
flowchart LR

Guest["비회원"]
Member["회원"]
Admin["관리자"]

subgraph "Tour Management"

A["관광지 목록 조회"]
B["카테고리별 검색"]
C["관광지 상세 조회"]

D["관광지 즐겨찾기"]

E["관광지 등록"]
F["관광지 수정"]
G["관광지 삭제"]

H["대표 이미지 조회"]
I["지도 이미지 조회"]
J["전화번호 조회"]
K["평균 별점 조회"]

end

Guest --> A
Guest --> B
Guest --> C

Member --> D
Member --> C

Admin --> E
Admin --> F
Admin --> G

%% include 관계
C -. "<<include>>" .-> H
C -. "<<include>>" .-> I
C -. "<<include>>" .-> J
C -. "<<include>>" .-> K

%% extend 관계 (즐겨찾기는 상세조회 확장 기능)
C -. "<<extend>>" .-> D
```

<br>

### 3. Review Management Use Case (GitHub 호환용)

```mermaid
flowchart LR

Member["회원"]
Admin["관리자"]

subgraph "Review System"

A["리뷰 작성"]
B["리뷰 수정"]
C["리뷰 삭제"]
D["내 리뷰 조회"]

E["부적절 리뷰 삭제"]

F["별점 선택 1~5"]
G["리뷰 내용 입력"]
H["작성자 본인 확인"]

end

Member --> A
Member --> B
Member --> C
Member --> D

Admin --> E

%% include 관계
A -. "<<include>>" .-> F
A -. "<<include>>" .-> G

B -. "<<include>>" .-> H
C -. "<<include>>" .-> H
```


<br><br><br>

**아래 mermaid 코드들은 현재 gitHub나 VS Code에서 usecaseDiagram를 지원하지 않아 출력이 되지 않고, 오류가 발생합니다.**



```mermaid
usecaseDiagram
title Tour & Review System Use Case Diagram

actor Guest as 비회원
actor Member as 회원
actor Admin as 관리자

rectangle "Tour System" {

Guest --> (관광지 목록 조회)
Guest --> (카테고리별 관광지 조회)
Guest --> (관광지 상세 조회)

Member --> (관광지 목록 조회)
Member --> (관광지 상세 조회)
Member --> (리뷰 작성)
Member --> (리뷰 수정)
Member --> (리뷰 삭제)

Admin --> (관광지 등록)
Admin --> (관광지 수정)
Admin --> (관광지 삭제)
Admin --> (리뷰 삭제(관리))

(관광지 상세 조회) ..> (리뷰 목록 조회) : <<include>>
(관광지 상세 조회) ..> (평균 별점 계산) : <<include>>

(리뷰 작성) ..> (별점 입력) : <<include>>
(리뷰 작성) ..> (리뷰 내용 작성) : <<include>>

(리뷰 수정) ..> (작성자 본인 확인) : <<include>>
(리뷰 삭제) ..> (작성자 본인 확인) : <<include>>

}
```

```mermaid
usecaseDiagram
title Tour Management Use Case

actor Guest as 비회원
actor Member as 회원
actor Admin as 관리자

rectangle "Tour Management" {

  Guest --> (관광지 목록 조회)
  Guest --> (카테고리별 검색)
  Guest --> (관광지 상세 조회)

  Member --> (관광지 즐겨찾기)
  Member --> (관광지 상세 조회)

  Admin --> (관광지 등록)
  Admin --> (관광지 수정)
  Admin --> (관광지 삭제)

  (관광지 상세 조회) ..> (대표 이미지 조회) : <<include>>
  (관광지 상세 조회) ..> (지도 이미지 조회) : <<include>>
  (관광지 상세 조회) ..> (전화번호 조회) : <<include>>
  (관광지 상세 조회) ..> (평균 별점 조회) : <<include>>

}

```


```mermaid
usecaseDiagram
title Review Management Use Case

actor Member as 회원
actor Admin as 관리자

rectangle "Review System" {

  Member --> (리뷰 작성)
  Member --> (리뷰 수정)
  Member --> (리뷰 삭제)
  Member --> (내 리뷰 조회)

  Admin --> (부적절 리뷰 삭제)

  (리뷰 작성) ..> (별점 선택 1~5) : <<include>>
  (리뷰 작성) ..> (리뷰 내용 입력) : <<include>>

  (리뷰 수정) ..> (작성자 본인 확인) : <<include>>
  (리뷰 삭제) ..> (작성자 본인 확인) : <<include>>

}

```


<br><br>

## 논리적 관계

① Tour — Review 관계
- 1 : N 관계
- 하나의 관광지는 여러 개의 리뷰를 가질 수 있다
- 하나의 리뷰는 하나의 관광지에만 속한다

```mermaid
erDiagram

    TOUR {
        BIGSERIAL id PK
        VARCHAR name
    }

    REVIEW {
        BIGSERIAL id PK
        BIGINT tour_id FK
        NUMERIC star_rate
    }
    TOUR ||--o{ REVIEW : has
```

- `||` → 반드시 하나 (1)<br>
- `o{` → 여러 개 (N)<br>
- TOUR `1 : N` REVIEW 관계<br>
- REVIEW의 `tour_id`는 TOUR의 PK를 참조하는 FK<br>


② Member — Review 관계
- 1 : N 관계
- 한 회원은 여러 개의 리뷰를 작성할 수 있다
- 하나의 리뷰는 한 명의 회원이 작성한다

```mermaid
erDiagram

    MEMBER {
        BIGSERIAL id PK
        VARCHAR username
    }

    REVIEW {
        BIGSERIAL id PK
        BIGINT member_id FK
        NUMERIC star_rate
    }

    MEMBER ||--o{ REVIEW : writes
```

- MEMBER 1 : N REVIEW 관계
- REVIEW의 member_id는 MEMBER의 PK를 참조하는 FK
- 한 회원이 여러 리뷰를 작성 가능
- 한 리뷰는 반드시 한 회원에 속함

<br>

### 정보공학적 ERD (Chen 스타일)

```mermaid
flowchart LR

%% =========================
%% 엔티티 (사각형)
%% =========================
Tour["Tour"]
Member["Member"]
Review["Review"]

%% =========================
%% 속성 (타원 표현)
%% =========================

%% Tour 속성
T1(("tour_id (PK)"))
T2(("name"))
T3(("category"))
T4(("address"))
T5(("tel"))
T6(("created_at"))

%% Member 속성
M1(("member_id (PK)"))
M2(("username"))
M3(("password"))
M4(("email"))
M5(("role"))
M6(("created_at"))

%% Review 속성
R1(("review_id (PK)"))
R2(("star_rate"))
R3(("comment"))
R4(("created_at"))

%% =========================
%% 관계 (마름모 표현)
%% =========================
Writes{"Writes"}
Has{"Has"}

%% =========================
%% 엔티티-속성 연결
%% =========================

Tour --- T1
Tour --- T2
Tour --- T3
Tour --- T4
Tour --- T5
Tour --- T6

Member --- M1
Member --- M2
Member --- M3
Member --- M4
Member --- M5
Member --- M6

Review --- R1
Review --- R2
Review --- R3
Review --- R4

%% =========================
%% 관계 연결
%% =========================

Member --- Writes
Writes --- Review

Tour --- Has
Has --- Review
```

<br><br>

## 물리적 ERD

```mermaid
erDiagram

TOUR {
  BIGSERIAL id PK
  ENUM category
  VARCHAR name
  VARCHAR thumb
  VARCHAR address
  VARCHAR tel
  TEXT comment
  VARCHAR traffic_map
  VARCHAR tour_map
  VARCHAR food_map
  VARCHAR bed_map
  TIMESTAMP created_at
}

MEMBER {
  BIGSERIAL id PK
  VARCHAR username
  VARCHAR password
  VARCHAR name
  VARCHAR email
  ENUM role
  TIMESTAMP created_at
}

REVIEW {
  BIGSERIAL id PK
  BIGINT tour_id FK
  BIGINT member_id FK
  TEXT comment
  NUMERIC star_rate
  TIMESTAMP created_at
}

TOUR ||--o{ REVIEW : has
MEMBER ||--o{ REVIEW : writes
```
