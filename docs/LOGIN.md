---
title: Login
---

## 必要なもの

AWS CLI
Session Manager Plugin
IAM user(ARN)
MFA
Access Key
Secret Access Key

ローカル環境のAWS CLIコンフィグ設定

``` zsh
aws configure
AWS Access Key ID [None]: (自身のIAMユーザーの「アクセスキーID」を入力)
AWS Secret Access Key [None]: (自身のIAMユーザーの「シークレットアクセスキー」を入力)
Default region name [None]: (自身のリージョンを入力、東京だったら「ap-northeast-1」を入力)
Default output format [None]: json

```



``` zsh
$ cat ~/.aws/credentials
[default]
aws_access_key_id = XXXXXXXXXXXXXXXXXXX
aws_secret_access_key = xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

$ cat ~/.aws/config
[default]
region = ap-northeast-1
output = json
```


 MFAを利用したAWS CLI経由でのAWSリソースアクセス認証

 ``` zsh
$ aws sts get-session-token --serial-number arn:aws:iam::123456789012:mfa/ssm-user --token-code XXXXXX
{
    "Credentials": {
        “AccessKeyId”: "access-key-id",
        “SecretAccessKey”: "secret-access-key",
        “SessionToken”: "temporary-session-token",
        “Expiration”: "expiration-date-time"
    }
}
 ```

AWS CLIコマンドからssm経由でセッションスタート

``` zsh
$ aws ssm start-session --target i-xxxxxxxxxxxxxxxxxxx --region ap-northeast-1

Starting session with SessionId: xxxxxxxxxxxxxxxxxxxxxxxxxx
sh-4.2$

```
