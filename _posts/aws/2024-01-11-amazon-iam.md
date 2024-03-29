---
layout: single
title: "Amazon IAM"
categories: [AWS]
tag: [aws, saa, iam, policy, json]
---

## IAM Policies Structure
[**Policy Structure (KR)**](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/iam-policy-structure.html) <br>
[**IAM JSON Policies elements Reference (KR)**](https://docs.aws.amazon.com/ko_kr/IAM/latest/UserGuide/reference_policies_elements.html)

```json
{
  "Version": "2012-10-17",         // policy 문법(언어)의 버전
  "Id": "S3-Account-Permissions",  // policy의 식별자 (*선택사항)
  "Statement": [                   // (* 필수)
    {
      "Sid": "FullAccess",         // statement의 식별자 (*선택사항)
      "Effect": "Allow",           // 접근 허용 여부; Allow/Deny
      "Principal": {               // 정책을 적용 받을 account, user, 혹은 role의 정보
        "AWS": [
          "arn:aws:iam::123456789012:root"
        ]
      },
      "Action": [                  // Effect에 기반해 허용 혹은 거부될 policy들
        "s3:*"
      ],
      "Resource": [                // Action에 대해 허용될 리소스들
        "arn:aws:s3::mybucket/*"
      ],
      "Condition": {               // 조건이 충족되는 경우에만 해당 policy를 적용
        "StringEquals": {
          "s3:x-amz-acl": [
            "my"
          ],
          "s3:x-amz-grant-full-control": [
            "1234"
          ]
        }
      }
    }
  ]
}
```

<br>

## IAM Role

**[AWS 서비스에 대한 권한을 위임할 역할 생성](https://docs.aws.amazon.com/ko_kr/IAM/latest/UserGuide/id_roles_create_for-service.html)**<br>
**[사용자에게 AWS 서비스에 역할을 전달할 권한 부여](https://docs.aws.amazon.com/ko_kr/IAM/latest/UserGuide/id_roles_use_passrole.html)**

IAM Role은 IAM User와 같지만, 실제 사람이 사용하도록 만들어진 것이 아니고, AWS 서비스에 의해 사용되도록 만들어졌다.

예를 들어, EC2 인스턴스를 만들었을 때, EC2 인스턴스는 AWS에서 어떤 작업을 수행하려고 할 수 있다. 그러기 위해서는 **EC2 인스턴스에 권한을 부여해야 한다.**

이를 위해 IAM Role을 만들어, 이들을 하나의 개체로 만든다.

그러면, EC2 인스턴스가 **AWS에 있는 어떤 정보에 접근하려고 할 때, IAM Role을 사용하게 될 것**이다.

<br>

## IAM Security Tools

1. **IAM Credentials Report** *(account-level)*
  - 계정에 있는 사용자와 다양한 자격 증명의 상태를 포함.
  - **보안 측면에서 어떤 사용자를 주목해야 하는지를 발견하는 데 도움**.
    - 예를 들면, 어떤 사용자가 비밀번호를 바꾸지 않았는지, 계정을 사용하는지를 파악할 때 매우 유용.
  
2. **IAM Access Advisor*** (user-level)*
  - 사용자에게 부여된 서비스의 권한과, 해당 서비스에 마지막으로 액세스한 시간이 포함.
    - 보통 최근 4시간 동안의 활동 내역이 보인다.
  - **최소 권한의 원칙을 따르려고 할 때, 매우 도움**이 되는 정보.
    - 예를 들어, IAM 사용자가 오랜 시간 사용하지 않는 권한을 삭제하여서 최소 권한의 원칙을 따를 수 있다.

<br>

### 가이드 라인 및 모범 사례

1. root 계정을 사용하지 말 것.
2. 한 명의 AWS 사용자 = 한 명의 실제 사용자.
  *(One physical user = One AWS user)*
3. 사용자를 그룹에 넣어 해당 그룹에 권한을 부여해, 그룹 수준에서 보안을 관리.
4. 비밀번호 정책을 강하게 유지할 것.
5. MFA *(Multi-Factor Authentication)* 를 사용할 것.
6. AWS 서비스에 권한 *(permissions)* 을 부여할 것.
7. CLI나 SDK를 사용할 경우, 반드시 Access Key를 만들어 사용할 것.
8. 계정의 권한을 감사(audit)할 때는, IAM 보안 도구를 사용할 것.
  - 보안 도구: IAM Credentials Report, IAM Acceess Advisor
9. **절대로 IAM 사용자와 Access Key를 공유하지 말 것.**