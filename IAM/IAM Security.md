# IAM Password Policy

Strong password(higher security for account)를 위한 Policy를 설정 가능
- 최소 길이
- 특정 character type 설정
    - 대문자
    - 소문자
    - 숫자
    - 특수 문자
- IAM User들이 password 변경을 Allow / Deny 할 수 있음
- 일정 시간마다 password 변경을 요구할 수 있음
- 변경 시 이전 password 사용 방지 가능


<br>
<br>
<br>

# MFA(Multi Factor Authentication)

password 와 security device를 결합한 다중 보안
- password를 도난 당해도 물리적 장치(security device)로 보안 유지 가능

Virtual Device MFA

- Google Authenicator(phone only)
- Authy(phone only)
- 하나의 장치에서 Multiple Tokens 지원

Universal 2nd Factor(U2F) Security

- 3rd party solution
- single security token으로 여러 명의 User를 지원

Hardware Key Fob MFA Device

- Gemato(3rd party)

Hardware Key Fob MFA Device for AWS GovCloud(US)

- SurePassID(3rd party)
- 미국 정부 클라우드 사용 중이라면 사용 가능

<br>
<br>
<br>

# IAM Security Tools

IAM Credential Report

- Account Level
    - permission을 부여받은 user가 사용하면 account에 대한 Credentials Report가 생성
- 모든 User와 User별 Credentials에 대한 Report

IAM Access Advisor

- User Level
    - permission을 부여받은 User가 다른 User의 Credentials 확인 가능
- User의 Service Permissions과 해당 Service의 Latest access time 확인 가능
- IAM Permission Policy를 revise하는 용도 로 사용하기 좋음
