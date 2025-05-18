# Terraform手順書
Terraformの使い方を記す

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
