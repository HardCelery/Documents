# Terraform手順書
Terraformの使い方を記す

## インストール手順
Git、tfenvをインストールする

Terraformをインストールする
https://www.guri2o1667.work/entry/2024/05/22/%E3%80%90Terraform%E3%80%91Windows10/11%E3%81%A7Terraform%E3%82%92%E5%88%A9%E7%94%A8%E3%81%99%E3%82%8B%E6%96%B9%E6%B3%95%E3%81%AB%E3%81%A4%E3%81%84%E3%81%A6%EF%BC%882024%E5%B9%B45%E6%9C%88%E7%89%88

## AWSと接続
AWS CLIにてawsと接続しておく
詳細は「aws_credentials.md」を参照


## Terraform実行ステップ
**1. 初期化（初回のみ）**
```bash
terraform init
```

**2. 差分確認（プランの表示）**
```bash
terraform plan
```

**3. 本番展開（インフラ作成）**
```bash
terraform apply
```
→yesを入力して作成

<br>

**環境の削除**
```bash
terraform destroy
```
