# EBS

Instance 실행 중 attach 가능한 network drive

- Instance가 Terminate 되어도 Persist Data
- binding AZ (Snapshot Copy를 통해 AZ 이동 가능)
- Multi Attach (EC2 하나에 여러 EBS)
- network drive로 latency 가능성이 존재
- EC2로부터 quickly detach 가능
- Provisioned Capacity(나중에 늘릴 수 있음)
- EC2에서 Delete on Termination 기능 활성화 가능
    - root volume은  default enable
    - other volume은 default disable
- 생성 시점부터 요금 부과
  
&Image - EBS


<br>
<br>
<br>

# EBS Snapshot

한 시점의 EBS volume back up

- Recommended : Snapshot capture 할 때 detach volume
- AZ, Region을 건너서 copy 가능 (capture & restore)
- Snapshot Archive
    - Snapshot을 standard → archive tier로 옮김
    - 최대 75% 더 저렴
    - 복원시 24 ~ 72 시간 필요
- Recycle Bin
    - AMI와 EBS를 delete시 Recycle Bin으로 이동
    - 보관은 Rule (1day ~ 1year)까지 가증
- Fast Snapshot Restore(FSR)
    - Force Full Initialization Snapshot
    - 첫 사용시 latency를 없앨 수 있음
    - 원래는 Snapshot으로 Block Data Reference를 S3에 저장하고 이를 참고해 점진적으로 load
    - FSR은 즉시 load
- 용량이 크거나, 성능이 중요한 경우 이용
- 생성 시점부터 요금 부과

<br>
<br>
<br>

# AMI (Amazon Machine Image)

EC2로 만든 Machine Image

- software, configuration, OS, monitoring
- software를 pre-package 해서 Fast Boot 가능
- 특정 Region에서 Build
- 다른 Region으로 Copy하여 이동 가능 (AWS Global Infra 활용)
- EC2를 실행 가능한 방법
    - public AMI : AWS Provided (ex: Linux)
    - own AMI : 자체제작 및 유지보수 AMI
    - marketplace AMI : 다른 사람이 build한 AMI

AMI Process
1. EC2 Customizing
2. stop Instance(for data integrity, 중단이 필수는 아니지만 일관성을 위해 필요)
3. AMI build (EBS Snapshot도 같이 생성됨)
4. AMI로 Instance 실행

<br>
<br>
<br>

# EC2 Instance Store

EC2 의 Physical hardware disk를 사용하는 휘발성 storage

- EBS 보다 빠름 (network drive가 아니므로)
- I/O 성능이 뛰어남
- EC2가 stop되면 소멸
- buffere, cache, scratch data, temporary content에 적합
- EC2 hardware 실패 시 data 손실 위험있음

<br>
<br>
<br>

# EBS Volume Types

- 6개의 volume type이 있음
- Size, Throughput, IOPS 를 기준으로 나눔
- g2, g3, io, io2 block express 만 root volume으로 사용 가능
- 대용량 트래픽 app의 경우 Snapshot 생성 시 유의(EBS의 IO를 사용해 S3에 Snapshot을 생성하므로 영항 발생)
- https://docs.aws.amazon.com/ebs/latest/userguide/ebs-volume-types.html


### General Purpose

특징

- cost effective
- low latency
- 1 GiB ~ 16 TiB

gp2

- Size에 따라서 IOPS, Throughput 이 결정됨
- 3 IOPS per GiB
- IOPS : 100 ~ 16,000 IOPS
- Throughput :  ~ 250 MB/s
- Burst Credit이 있을 경우 최대 3,000 IOPS까지 Burst
- 5334 GiB면 최대 IOPS 도달

gp3

- Size, IOPS, Throughput 독립적으로 increase 가능
- IOPS : 3,000 ~ 16,000
- Throughput : 125 ~ 1000 MB/s

### Provisioned IOPS (PIOPS) SSD

특징

- 지속적인 IOPS 성능이 중요한 Application
- 16,000 IOPS 이상이 필요한 경우
- Database workload (storage performance와 consistency가 중요한 경우)

io1

- 4 GiB ~ 16 TiB
- Nitro EC2 PIOPS : ~ 64,000
- Other : ~ 32,000
- PIOPS를 Size와 별도로 increase 가능

io2 Block Express

- 4GiB ~ 64 TiB
- Sub-millisecond latency
- PIOPS : ~ 256,000
- 상한 규칙은 IOPS:GiB ratio of 1,000:1
- EBS Multi-Attach 지원

### Hard Disk Drive (HDD)

특징

- boot volume으로 사용 불가
- 125 GiB ~ 16 TiB

st1 (Throuput Optimized HDD)

- 빅데이터, 로그 처리, 대규모 순차 읽기/쓰기 워크로드
- Throughput : ~ 500 MiB
- IOPS : ~ 500

sc1 (Cold HDD)

- infrequently accessed data 용
- Archive Data
- Throuput : ~ 250 MiB
- IOPS : ~ 250

<br>
<br>
<br>


# EBS Multi-Attach

같은 EBS를 같은 AZ 내의 여러 EC2에 연결

- io1 / io2 famliy만 가능
- 최대 16개 EC2 가능
- 각 EC2가 full read & write permission을 가짐
- Use Case
    - Clustered Linux Application 고가용성 향상 (ex : TeraData)
    - concurrent write operation을 관리해야할 때
- Cluster-Aware File System을 사용해야함
  
&Image - MultiAttach

<br>
<br>
<br>

# EBS Encryption

EBS voulme 암호화

- encrypted EBS volume 생성시 encryption 되는 것
    - volume안 Data
    - instance와 volume 사이의 모든 flight moving data(전송 데이터)
    - 생성하는 snapshot
    - snapshot으로 생성한 EBS
- Encryption, Decryption은 transparently 처리됨 (보이지 않게 처리 되므로 직접 처리할 것은 없다)
- latency에 거의 영향을 주지 않음 (minimal impact)
- KMS에서 key를 생성함 (AES-256)
- unencrypted snapshot은 복사본을 만들어 encrypt

unencrypted EBS volume 을 encrypt하는 방법

1. EBS volume snapshot 생성
2. using copy로 encrypt snapshot 생성
3. encrypt snapshot으로 ebs volume 생성 (encrypt volume)
4. ebs volume을 instance에 attach

<br>
<br>
<br>

# EFS

EC2에 mount되눈 managed Network File System

- Multi-AZ 의 EC2 와 연결 가능
- High Availibility
- High Scalibility
- expensive (gp2 의 3배 가격)
- pay per use
- scales automatically (no capacity planning)
- NFSv4 protocol 사용
- POSIX file System 사용
- EFS에 access하려면 Security Group이 필요
- Linux based AMI만 호환됨 (not Windows)
- Encryption at rest (using KMS)
- standard file API 있음
- Use case
    - content management
    - web serving
    - data sharing
    - Wordpress
 
&Image - EFS

### EFS Scale

- 동시에 수천개의 NFS 클라이언트 연결, 10 GB/s 의 Throughput 확보
- Peta Byte 규모의 NFS로 자동 확장

### Performance Mode

EFS 생성시 Performance Mode를 설정 가능

- General Purpose
    - low latency
    - web setver, CMS
- Max I/O
    - higher latency
    - high Throughput, parallel
    - big data, media processing
- Throughput Mode
    - Bursting - 1 TB : 50 MiB/s + burst up to 100 MiB/s
    - Provisioned : Storage와 관계없이 미리 Throughput 설정
    - Elastic : automatically scale, throughput을 예측하기 어려운 workload에 적합

### Storage Class

Storage Tiers

- Lifecycle Management featrue, 몇일 후 파일을 다른 Tier로 옮긴다
- Lifecycle Policy를 구현해서 Tier 이동을 정의
- Tier 종류
    - Standard : frequently access
    - Infrequently Access (EFS-IA) : 저장 비용 감소, 검색 시 비용 발생
    - Archive: rarely accessed, 50% cheaper

Availibility & Durability

- Standard : Multi AZ
- One Zone : One AZ, enable Back up
- IA와 호환 (EFS Standard IA, EFS One Zone IA)

올바른 Storage class 사용시 최대 90% 비용 감소
