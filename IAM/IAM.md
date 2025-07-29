
# IAM(Identity Access Management)

AWS 계정을 관리하는 서비스

- User를 관리(생성/삭제)
- User를 그룹에 배치
- Global Service: Region에 지정없이 전역에서 동작
- Root Account는 default로 생성됨

<br>
<br>
<br>

# IAM User Group

User를 그룹에 배치할 수 있음

- Group은 User만 포함 가능, 다른 Group은 포함 불가
- User는 여러개의 Group에 배치 가능
- User를 Group에 배치하지 않아도 됨(Not Recommended)

<img width="377" height="145" alt="iam-group drawio" src="https://github.com/user-attachments/assets/0545adcf-79c7-42aa-a715-40d55fa8eaea" />

<br>
<br>
<br>

# IAM Permission Policy

Policy를 생성하여 User or Group에 permission을 부여할 수 있음

- Json Document로 policy를 생성
- 보안과 비용 과다 문제 방지를 위해 least privilege principal(최소 권환 정책 적용)
- Policy는 Principal 설정에 따라 크게 두 종류가 있음
    - Identity-Based : User나 Group에 붙는 정책, Principal 설정 x
    - Resource-Based : Resource에 붙는 정책, Principal 설정 o
<img width="491" height="47" alt="iam-policy drawio" src="https://github.com/user-attachments/assets/092588fa-d5a2-47ce-ba0c-c9e8f51e8474" />

<br>
<br>
<br>

# Policy Json Document

Json Document Example

- AdministratorAccess
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "*",
            "Resource": "*"
        }
    ]
}

```

- AmazonEC2FullAccess
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": "ec2:*",
            "Effect": "Allow",
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "elasticloadbalancing:*",
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "cloudwatch:*",
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "autoscaling:*",
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "iam:CreateServiceLinkedRole",
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "iam:AWSServiceName": [
                        "autoscaling.amazonaws.com",
                        "ec2scheduled.amazonaws.com",
                        "elasticloadbalancing.amazonaws.com",
                        "spot.amazonaws.com",
                        "spotfleet.amazonaws.com",
                        "transitgateway.amazonaws.com"
                    ]
                }
            }
        }
    ]
}

```

Document

| Name | Description | Required |
| --- | --- | --- |
| Version | Document의 버전 | Required |
| Id | Document의 아이디 | Optional |
| Statement | 상세 설정 | Required |

Statement

| Name | Description | Required |
| --- | --- | --- |
| Sid | Statement의 Id | Optional |
| Effect | Allow, Deny로 어떤 영향을 주는지 | Required |
| Principal | 영향받는 account,user,group,role | Optional |
| Action | 영향을 받는 API | Not Action, Action 중 하나 Required |
| Resource | 적용할 AWS Resource | Optional, 미작성 시 “*”로 모든 Resource |
| Condition | 적용되는 시간 | Optional |


<br>
<br>
<br>

# IAM Roles

AWS의 services에 대한 permissions을 부여

- Examples
    - EC2 Instance
    - Lambda Fuction
    - CloudFormation
      
<img width="381" height="115" alt="iam-role drawio" src="https://github.com/user-attachments/assets/983eb3c6-2ec2-43af-aba8-59ee495fc0ac" />


