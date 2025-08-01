# IP

네트워크에서 machine 을 식별하기위한 고유 장치

<br/>

**Public IP**

- Machine이 인터넷 범위에서 식별됨을 의미
- 전체 Web에서 유일함
- 쉽게 지리적 위치를 찾을 수 있음

<br/>

**Private IP**

- Private 네트워크에서만 식별됨
- Private 네트워크에서만 Unique
- 서로 다른 Private 네트워크가 2개면 같은 Private IP 존재 가능
- NAT 장치와 Internet Gateway를 통해 인터넷(WWW)에 연결 가능함
- 지정된 범위 안에서만 Private IP를 사용 가능
- Defualt
    - private IP : inernal AWS Network
    - Public IP : WWW
    
<br/>

**Elastic IP**

Fixed pulic IP가 필요할 때 사용하는 IP
- EC2는 stop/start 과정에서 public IP가 변할 수 있음
- IPv4
- 삭제하지 않으면 계속 유지됨
- 한번에 하나의 Instance에만 attach
- 5 Elstic IP per Account
- EC2간 이동하여 attach 가능
- Elastic IP 보다 다음이 권장됨
    - 임의의 공용 IP와 DNS name 할당
    - Load Balacer를 통해 pulic IP 사용 x

<br/>
<br/>
<br/>

# Elastic Network Interface(ENI)

EC2 용 Virtual Network Card
- VPC의 논리적 구성 요소
- EC2가 네트워크에 접속 가능하도록 함
- EC2 생성시 primary ENI(eth0)이 생성됨
- EC2가 Security Group에 설정되는건 ENI를 통해 이루어 짐
- ENI는 Subnet에 고정 됨
    - VPC는 Region에 여러 AZ에 있는 Subnet으로 구성
    - Subnet은 하나의 AZ binding
    - ENI는 Subnet에 binding, 이후 변경 불가
- Attributes
    - primary Private IP
    - 1 Public IP (primary Private IP와 mapping)
    - 1 or more secondary IP
    - 1 Elastic IP per Pirvate IP
    - 1 or more Security Group
    - A MAC Address
- EC2와 별도로 생성하고 attach/detach 가능
- 장애 대응을 위해 ENI를 이동 가능(primary ENI 제외)
