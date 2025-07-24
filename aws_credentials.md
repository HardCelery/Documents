## ローカルでAWS CLIを利用するとき

ローカルでAWS CLIを利用するときは認証が必要
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
