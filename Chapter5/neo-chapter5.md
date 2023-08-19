# 5.1 Redis 아키텍처

## 1. 메모리 영역

1. **Resident Area**
   - 사용자의 작업이 먼저 저장되는 영역
   - 실제 작업 수행 공간
   - "WorkingSet" 영역으로도 표현

2. **Data Structure**
   - 상태 정보 저장을 위한 메모리 공간
   - Redis Server 운영 및 모니터링에 필요

## 2. 파일 영역

1. **AOF 파일**
   - 중요한 데이터 지속 저장
   - 스냅샷 데이터

2. **DUMP 파일**
   - 소량 데이터 일시적 저장

## 3. 프로세스 영역

1. **Server Process**
   - Redis 인스턴스 관리 및 사용자 작업 수행
   - 4개의 멀티 쓰레드로 구성
   - Main thread, Sub thread 1(BIO-Close-File), Sub thread 2(BIO-AOF-Resync), Sub thread 3(BIO-Lazy-Free)

2. **Client Process**
   - 사용자 애플리케이션을 위한 프로세스
   - 분산/복제 가능한 클러스터 환경에서 추가적인 프로세스 유형도 고려

## 5.2 시스템 & Disk 사양

### 최소 사양

- 노드 수: 3대 이상 (클러스터 시 3개 이상)
- 코어 수: 4개 이상 (8개 이상 권장)
- RAM: 15GB 이상 (30GB 권장)
- 스토리지: SATA (Redis on Flash 모드는 RAM 크기 x 10)
- Persistent Storage: RAM 크기 x 3 (>= RAM 크기 x 6 또는 >= RAM + Flash 크기 x 5)
- Network: 1G (>= 10G 권장)

### 권장 사양

- 노드 수: >= 3대 (클러스터 시 3개 이상)
- 코어 수: >= 8개
- RAM: >= 30GB
- 스토리지: SSD
- Persistent Storage: (>= RAM 크기 + Flash) x 5
- Network: >= 10G

## 5.3 메모리 운영 기법

- **LRU(Least Recently Used) 알고리즘**
  - 메모리에 최근 데이터 재배치
  - Redis Server 성능 향상

- **LFU(Least Frequently Used) 알고리즘**
  - 메모리에 자주 사용되는 데이터 재배치
  - Redis Server 성능 향상
  - 파라미터 설정 필요

## 5.4 LazyFree 파라미터

- LazyFree 파라미터 설정
  - unlink로 삭제 시 백그라운드 작업으로 처리
  - 메모리 관리 최적화

## 5.5 데이터 Persistence

- RDB 파일 저장 방법
  - SAVE 명령어나 interval에 따라 주기적 저장
  - snapshot 형태로 저장

- AOF 파일 저장 방법
  - appendonly.aof 파일로 저장
  - 데이터 수정 시 자동 저장

## 5.6 Copy on Write

- 데이터 복제 시 Copy on Write 발생
- Redis 성능 지연 가능
- Redis Server 메모리 확보 필요
- 다양한 상황에서 발생 가능
