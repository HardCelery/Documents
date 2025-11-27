# AWSの認証方式について

AWSの認証方式には主に5つある。
ここではそれぞれの特徴と用途、メリット・デメリットを説明する。

<br>

## ①IAMユーザーのAccess Key認証
　もっとも古くからある認証方式であり、IAMユーザーそれぞれに付与される以下２点を利用して認証を行う。

・AccessKeyId
・SecretAccessKey

### メリット
・セットアップが簡単
・小規模環境で使いやすい

### デメリット
・固定のキーなので運用（ローテーション・削除）を適切に行う必要がある。
・現在では非推奨となった方式

<br>

# ②IAMロールによる一時認証


# ③AWS IAM Identity Center（旧AWS SSO）

**AWS IAM Identity Center（旧 SSO）** を使ったCLI認証を行う。
IAMユーザーでの認証だとアクセスキー等を記載するので非推奨となる。
`c:\users\user\ .aws\config`
上記のconfigに情報を記載

```
[profile developer]
sso_session = my-sso
sso_account_id = 358353272915
sso_role_name = AdministratorAccess
region = ap-northeast-1
output = json
[sso-session my-sso]
sso_start_url = https://d-9567b0dc01.awsapps.com/start
sso_region = ap-northeast-1
sso_registration_scopes = sso:account:access
```

情報が含まれている状態で
```
aws sso login --profile "developer"
```
でログインする。（プロファイル名は任意）

ログインしてもawsコマンドは使えず、その都度プロファイルを指定する必要がある。
面倒なので環境変数（Windows）にセットしておく
```
$env:AWS_PROFILE = "developer"
aws sso login
```
これでaws cliが使えるようになる。
ただしvscodeでは再起動するとキャッシュされたトークンを認識できなくなり再ログインが必要
