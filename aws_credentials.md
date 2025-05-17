## ローカルでAWS CLIを利用するとき

ローカルでAWS CLIを利用するときは認証が必要
**AWS IAM Identity Center（旧 SSO）** を使ったCLI認証を行う。

c:\users\user\ .aws\config
上記のconfigにアクセスキー、シークレットキーなどの情報が含まれている

情報が含まれている状態で
```
aws sso login --profile "developer"
```
でログインする。（プロファイル名は任意）

ログインしてもawsコマンドは使えず、その都度プロファイルを指定する必要がある。
面倒なので環境変数（Windows）にセットしておく
```
[System.Environment]::SetEnvironmentVariable("AWS_PROFILE", "developer", "User")
```
これでaws cliが使えるようになる。
