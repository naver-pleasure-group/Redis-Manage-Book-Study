# 6.1 복제 & 분산 시스템 개요

## 자원 공유
- 서버 자원 제한적, 과부하 시 다른 서버 자원 활용으로 효율성 증가

## 성능 향상
- 과부하 발생 시 로드 밸런싱을 통해 자원 분배 가능

## 안정성
- 복제 서버로 장애 대체하여 작동
- 데이터 참조 가능해 위험 분산 가능
- Redis: 마스터-슬레이브, 마스터-슬레이브-센티널, 파티션 클러스터로 복제, 분산처리 가능

## Partition 유형
### 범위 파티션 (Range Partition)
- Key-Value를 서버에 분산 저장
- 서버 대수 결정에 따라 저장 위치 결정
- 서버에 저장할 데이터 양 불균형 문제 발생 가능

### 해시 파티션 (Hash Partition)
- Hash 알고리즘으로 데이터 분산 저장

## Partition 구현 방법
### Client Side Partitioning
- 사용자가 데이터 분산 설계 결정
- Redis: Master-Slave, Redis Cluster Data Sharding로 구현

### Proxy Assisted Partitioning
- Proxy Server 사용, 데이터 저장 결정
- 사용자 데이터 요구 시 적절한 서버로 저장
- Redis: Twemproxy Cluster와 연동 구축

### Query Routing
- 해시 파티션으로 데이터 분산 저장
- 장애 시 Slave 서버 사용 가능해 읽기 작업 유지

# 6.2 Master & Slave & Sentinel

## 시스템 설정
### Master-Slave 복제 시스템
- Master 서버 데이터 실시간 복제
- Slave 서버는 읽기 작업만 가능
- Master 장애 시 Slave로 대체 가능

### Master-Slave-Sentinel 복제 시스템
- Slave로 읽기 작업만 가능, 쓰기 작업 불가
- Sentinel 서버 추가로 구축해 Failover 지원

## 장애 처리 방법
- Sentinel 서버 모니터링, Master 서버 장애 시 Slave 서버로 Failover
- Sentinel 서버 리더 선출해 결정
- Slave 서버 Master로 승격, 다른 Slave 모니터링

# 6.3 부분 동기화
- Master 서버 장애 시 Slave 서버 데이터 복제
- Redis 4.0부터 repl-backlog-size 파라미터로 부분 동기화 가능


## 6.4 Redis 클러스터 구성과 운영

### 6.4.1 클러스터 서버

대용량 데이터 생성 환경에서는 단일 독립 서버로만 성능 문제와 장애가 발생할 수 있습니다. 이를 해결하기 위해:

1. **Scale-Up**: 서버의 하드웨어 자원을 업그레이드하되 이 방법에는 한계가 있습니다.
2. **Scale-Out**: 데이터를 분산하기 위해 별도의 서버를 추가하며, 이는 파티셔닝 및 복제 시스템을 통해 이루어집니다. 이러한 환경에서는 Sentinel 서버가 필요하지 않습니다. 대신 Redis 클러스터(샤드-복제)를 사용합니다.

### Redis 클러스터 특징

- Redis 3.0에서 소개되었습니다.
- 오직 데이터베이스 0을 사용합니다.
- 데이터 표현 및 분산을 위해 해시 태그를 지원합니다.
- Master 및 Slave 서버만으로 구성되며, Sentinel 서버가 필요하지 않습니다.
- 데이터 분산을 위해 기본적으로 16,384 슬롯을 사용합니다.

### Redis 클러스터 설정 방법

1. **클러스터 명령어로 수동 설정**: 사용자가 물리적 설계를 직접하며, 클러스터에 대한 최적 제어를 제공하며, 숙련된 관리자에게 적합합니다.
2. **redis-trib.rb 유틸리티로 자동 설정**: 자동화된 설정 알고리즘을 제공하며 기술적 이해가 제한적인 사용자에게도 적합하며, 장애 시 자동 시스템 재구성을 수행합니다.

### Redis 클러스터 노드 구성

- 데이터 분산을 위해 최소 3개의 Master 서버가 필요하며, 각 Master에 대비하여 Slave 서버를 구축해야 합니다.
- Master와 Slave의 비율은 1:1일 필요는 없습니다.
- 수동 설정은 균형을, 유틸리티는 자동화된 구성을 제공합니다.

### Redis 클러스터 파라미터

- `CLUSTER-ENABLED`: Redis 클러스터 모드를 활성화하려면 YES로 설정합니다.
- `CLUSTER-CONFIG-FILE`: 클러스터 노드 상태를 기록하는 파일의 경로와 파일명을 지정합니다.
- `CLUSTER-NODE-TIMEOUT`: 클러스터 노드가 다운된 것을 판단하기 위한 타임아웃 시간을 설정합니다 (밀리초 단위).

### Redis 클러스터 고가용성 (HA) 구성

- Redis 클러스터는 여러 서버에 데이터를 분산하여 고가용성 구성을 지원합니다.
- 데이터 분산은 입력된 키에 CRC16 함수를 적용하여 슬롯 값에 따라 이루어집니다.
- 기본 설정은 슬롯을 노드에 분산합니다.
- `CLUSTER-ENABLED`, `CLUSTER-CONFIG-FILE`, `CLUSTER-NODE-TIMEOUT` 등의 파라미터는 Redis 클러스터 설정에 중요합니다.
