# 4.1 키-밸류(Key-Value) 데이터 모델링 개념

## 4.1.1 용어 설명

- **테이블(Table)**: 관계형 DB에서 데이터를 저장하는 논리적 구조를 테이블(table)이라고 표현하는 것처럼 Key-Value DB에서도 테이블(Table)이라고 함.
- **필드(Field) 또는 엘리먼트(Element)**: 하나의 테이블을 구성하고 있는 요소들을 관계형 DB에서는 컬럼(column)이라고 하는데 Key-Value DB에서는 필드(Field) 또는 엘리먼트(Element)라고 말함.
- Key-Value DB에서는 하나의 Key와 하나 이상의 필드 또는 엘리먼트로 구성됨.
- Key-Value DB에서는 제약 조건 기능이 제공되지 않지만, HyperLogLogs 데이터 속성을 통해 원하는 조건의 데이터를 저장, 관리할 수 있음.

## 4.1.2 Redis 데이터 모델링 가이드라인

### 1. Hash 데이터 모델

Redis의 Hash 데이터 모델과 가장 유사한 관계형 DB의 데이터 모델은 부모-자식 테이블에 해당됨. 데이터 관계를 Redis DB에서 구현할 때 Hash 모델을 선택하면 됨.

### 2. List 데이터 모델

Redis의 List 데이터 모델과 가장 유사한 관계형 DB의 데이터 모델은 마스터-디테일 테이블에 해당됨. List 타입은 객체지향 프로그래밍 언어에서 사용되는 배열 구조와 유사함.

### 3. Set/Sorted Set 데이터 모델

Redis의 Set/Sorted Set 데이터 모델과 가장 유사한 관계형 DB의 데이터 모델은 계층형(Tree Structure) 테이블에 해당됨. 관계형 DB에서는 Self-Reference 관계 테이블이라고 표현함.

### 4. HyperLogLogs 데이터 모델

Redis DB의 HyperLogLogs 데이터 모델과 가장 유사한 관계형 DB의 데이터 모델은 Check 제약조건에 해당됨. 일반적으로 해당 컬럼에 저장할 데이터가 원치 않는 데이터가 입력 및 변경되는 것을 방지하기 위한 목적으로 사용됨.

## 4.1.3 데이터 모델

Redis Server에서 설계할 수 있는 데이터 모델 유형:

- Hash-Hash Data Model
- Hash-List Data Model
- List-List Data Model
- Set/Sorted Set-List Data Model
- HyperLogLogs Data Model

# 4.2 논리적 DB 설계

1. 하나의 Redis 서버는 여러 개의 데이터베이스로 구성됨.
2. 스트라이핑(Striping) 기법을 통해 데이터의 안전한 저장 및 관리를 원칙으로 함.
3. 데이터의 백업 및 복구 측면에서 하나의 DB로 생성하는 것보다 여러 개의 DB로 분리 생성하는 것이 유리함.
4. 분산 Database Lock을 지원하므로 보다 세분화된 DB 설계가 필요함.

# 4.3 SwapDB 설계

1. Redis 4.0.0 버전부터 swapDB 기능을 제공함.
2. 모든 클라이언트는 swapDB를 사용하여 다른 Local DB 데이터를 빠르게 처리할 수 있음.
3. swapDB는 데이터 처리 지연 문제를 해결하기 위한 기능으로, 운영체제의 swap 메모리와 유사한 개념을 가지고 있음.
