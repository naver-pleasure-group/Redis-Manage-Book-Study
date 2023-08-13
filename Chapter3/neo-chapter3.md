## 3.1 Isolation & Lock

1. 모든 NoSQL 제품이 트랜잭션을 제어하지는 않지만, Redis는 트랜잭션 제어 가능.

2. Redis는 Read Commited 타입의 트랜잭션 제어도 가능함.

3. Redis 4.0 버전에서 Data Sets 락 매커니즘 제공.

## 3.2 CAS (Check and Set)

1. CAS는 데이터 일관성 공유를 위한 기술로 동시 처리 시 충돌을 피함.

2. Watch 명령어를 사용하여 충돌 감지 가능.

## 3.3 commit & rollback

1. EXEC: 변경한 데이터 최종 저장에 사용.

2. DISCARD: 변경한 데이터 최종 저장하지 않고 취소.

## 3.4 Index 유형 및 생성

1. Redis는 Primary Key Index와 Secondary Key Index를 제공.

2. Exact Match와 Range By Secondary Index로 검색 가능.

## 3.5 사용자 생성 및 인증/보안/Roles

1. Redis는 다양한 액세스 권한과 인증 방법 제공.

2. OS 인증 방법과 Internal 인증 방법 제공.

3. 액세스 컨트롤 권한은 사용자 계정과 비밀번호를 통해 제어.

4. Internal 인증 방법은 auth 명령어로 사용자 인증.

5. Enterprise Edition에서 사용자 생성 및 Role 기능 제공.
