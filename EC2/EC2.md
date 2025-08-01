# EC2 (Elastic Compute Cloud)

Infrastructure as a Service

원하는대로 AWS로부터 Virtual Machine을  빌릴 수 있음

- 다음의 서비스를 포함
    - EC2(Elastic Compute Cloud)
    - EBS(Elastic Block Storage)
    - ELB(Elastic Load Balancer)
    - ASG(Auto Scaling Group)
- Congiguration Options
    - OS : Linux, Window, Mac
    - CPU
    - RAM
    - Storage :  EBS, EFS, EC2 Instance Store
- Network card : Sspeed Public IP
- Security Group
- User Data

<br/>
<br/>
<br/>

# EC2 Instance Types

EC2 Instance를 사용 목적에 최적화된 Type으로 설정할 수 있음

https://aws.amazon.com/ec2/instance-types/

**Instance Types Naming Convention**

&이미지Naming Convention

<br/>

**Instance Types - General Purpose** 

범용 워크로드, 다양한 작업에 적합(Web server, Code repositories)

- CPU, Memory, Networking이 밸런스가 잘 맞음

<br/>

**Instance Types - Compute Optimized**

high performance processor 를 요구하는 compute-intensive task에 최적화 됨

- Batch
- Media transcoding
- Highperformance Web Server
- HPC (High Performance Computing)
- Sientific Modeling
- Machine Learning
- Dedicated Gaming Server

C로 시작함

<br/>

**Instance Types - Memory Optimized**

Memory(RAM)에서 large data sets fast process 하는데 최적화

- In-Memory Database
- Distributed web scale cache stores
    - Elastic Cache
- BI (Business Intelligence) Database
- 대규모 unstructured data(비정형 데이터) 실시간 처리 Application

R, X, Z로 시작함

<br/>

**Instance Types - Storage Optimized**

Local Storage에서 large data set에 access 최적화

- OLTP (High Frequency Online Transaction Processing)
- Database (Relational & NoSQL)
- In-Memory Database
    - Redis
- Data warehousing Application
- Distributed File System

I, D, H 로 시작함

<br/>

**전체 Instance 비교**

[https://instances.vantage.sh](https://instances.vantage.sh/)

<br/>
<br/>
<br/>

# EC2 Security Group

EC2의 inbound, outbound traffic을 제어하는 외부 firewall 역할

- only Allow rules
- Security Group 은 IP, Security Group을 참조함
- Regulate
    - Port
    - Authorized IP Ranges (IPv4, IPv6)
    - inbound network traffic
    - outbound network traffic
- Multiple Instance에 attach 가능
- Region/VPC 에 제한
- 외부 firewall로 내부에서는 확인 불가능
    - connection refused : security group issue
    - time out : application issue
- Defualt
    - Allow all outbound
    - Not Allow all inbound

&Security Group Image

<br/>
<br/>
<br/>

# EC2 Instances Purchasing Options

EC2를 workload 에 맞춰 합리적으로 구매하기위한 Options

<br/>

**Purchasing Options - On Demand**

사용한만큼 청구하는 방식

- 가장 높은 요금, 선불 x , 약정 x
- short-term & uninterrupted workload 또는 예측 불가능한 Application에 적합
- Linux, Window : billing per second, after first minute
- Other OS : billing per hour

<br/>

**Purchasing Options - Reserved Instances**

특정 Instance Attributes를 미리 예약하는 방식

- On Demand 보다 저렴
- Instance Attributes 를 미리 예약
    - Region
    - Tenancy
    - Instance Type
    - OS
    - etc..
- Reservation Period
    - 1 year (+discount)
    - 3 year (++discount)
- Payment Options
    - No Upfront (+discount)
    - Partitial Upfront (++discount)
    - All Upfront (+++discount)
- Reservation Scope
    - Region
    - Availability Zone
- Marketplace에사 Reserved Instance를 사고 팔 수 있음
- Convertible Reserved Instance : Instance Attribute를 변경 가능, 할인폭 적음

<br/>

**Purchasing Options - Saving Plan**

일정 기간동안 시간 당 사용량을 약정하는 방식

- 시간 당 사용량을 넘으면 On Demand 요금 청구
- 약정 기간
    - 1 year
    - 3 year
- 특정 Instance Family와 Region에 고정
- Fixible across
    - Instance size
    - OS
    - Tenancy

<br/>

**Purchasing Options - Dedicated Hosts**

Instance의 실제 물리적인 전용 서버를 제공 받음

- Compliance Requirement, Server-Bound software Lincense (per core, per socket, per VM software) 를 위해 사용
- BYOL(Bring Your Own License)인 경우 사용
- 강력한 규정이나 법규를 준수해야하는 경우 사용
- Purchasing Options
    - On Demand
    - Reserved (1 or 3 years, No Upfront, Pratitial Upfront, All Upfront)

<br/>

**Purchasing Options - Dedicated Instances**

전용 논리적인 서버를 제공 받음

- 즉 물리적인 서버가 다른 사람과 공유되지 않지만 실제 물리적인 서버를 제공 받는것은 아님
- Stop/Start 이후 물리적인 서버는 변경될 수 도 있음
- 같은 계정의 다른 Instance와 공유 가능

<br/>

**Purchasing Options - Capacity Reservations**

기간동안 On Demand Instances의 Capacity를 특정 AZ에 미리 예약하는 방식

- 특정 AZ에 있어야하는 short-term, uninterrupted workload에 적합
- Capacity 내에서 언제든 On Demand에 access 가능
- 시간 제한 없음
- 할인 없음
- 할인을 받으려면 Region의 Reserved Instances 또는 Saving Plan과 결합해야 함
- Run과 상관없이 예약한 On Demand 요금이 청구됨

<br/>

**Purchasing Options - Spot Instances**

AWS의 남는 자원을 경매하야 Instance를 유지하는 방식

- 가장 저렴함
- 지불하려는 최대 가격을 정의하고 넘을 경우 손실 됨
- resilient to failure workload에 적합
    - Batch
    - Data Analysis
    - Image Processing
    - Distributed workload
    - 시작과 끝 시간이 정해진 workload
- 중요한 작업, Database에는 적합하지 않음
- 최대 가격 초과시 2분의 유예시간내에 선택
    - Stop : 다시 최대 가격보다 내려가면 중단 지점부터 Restart
    - Terminate : 완전 종료
- Spot Block을 통해 일정 시간동안 Spot Instance를 차단하여 중단없이 실행 가능(1 ~ 6 hours)
    - 드물게 회수되는 경우도 있음
- 고려중인 AZ에 따라서 요금이 다름

<br/>
<br/>
<br/>

# EC2 User Data

부트스트래핑을 지원하는 EC2 script
- Instance의 first start에서 한번만 실행됨
- automate boot tasks such as
    - update
    - install software
    - common file download
    - etc.
- sript로 실행하는 작업아 많을수록 boot 속도는 느려짐
- Root user에서 실행됨

<br/>
<br/>
<br/>

# EC2 Hibernate

EC2의 RAM에 In-Memory state를 유지한채로 stop 하는 방식
- stop / terminate → restart 보다 boot 속도 빠름
- In-Memory state는 root EBS에 dump
    - root EBS only
    - EBS volume enrypted
    - EBS 용량이 충분해야함
- Supported Instance Family에서만 이용 가능하지만 많음
- 최대 RAM은 150GB
- bare metal Instance는 사용 불가
- 많은 AMI / OS 에서 이옹 가능
    - Linux
    - Window
    - etc.
- Reserved Instance / Saving Plan / Spot Instance와 사용 가능
- 최대 60일까지 유지 됨
