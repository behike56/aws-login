---
title: 仕様書
version: 0.1
---

## Flow

1. configの確認
2. MFAを利用した認証
3. セッションスタート

### 1.configの確認

1-1. 「~/.aws/credentials」ファイルに「aws_access_key_id」と「aws_secret_access_key」が設定されているかを確認する。

#### credentialsファイル内容例

``` text
[default]
aws_access_key_id = XXXXXXXXXXXXXXXXXXX
aws_secret_access_key = xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

1-2. 「~/.aws/config」ファイルに「region」と「output」が設定されているかを確認する。

#### configファイル内容例

``` text
[default]
region = ap-northeast-1
output = json
```

### 2.MFAを利用した認証

2-1. UserARN、MFAデバイス名の入力を求める。
2-2. MFAの6桁のコードの入力を求める。
2-3. 2-1, 2-2の情報を元に認証を実行する。
2-4. 認証が成功した場合:
    2-4-1. 認証が成功した旨を表示する。
2-5. 認証が失敗した場合:
    2-5-1. 2-1からやり直す。

なお、認証情報の有効期限はデフォルトでは12時間となりますが、「--duration-seconds」オプションを使用することで、「900秒(15分)」から「129600秒(36時間)」まで変更可能です。ただし、rootユーザーの場合は「900秒(15分)」から「3600秒(1時間)」となります。

```text
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

### 3.セッションスタート

オプション

EC2へ接続する。
