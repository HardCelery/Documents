# AWSクライアントVPNによるリモート接続環境の構築
AWSクライアントVPNサービスを使ってプライベートサブネットに配置してあるEC2との疎通確認を行う。

## AWSサービス
・EC2
・VPC
・クライアントVPNエンドポイント
・Certificate Manager
・Cloud Watch

## クライアントVPNエンドポイントについて
クライアント端末からAWS上のVPCにセキュアに接続するためのVPNサービス
VPCに関連付けて使用する

**承認ルール**にて送信先CIDR（ネットワークアドレス）を指定する

認証には「相互認証」と「ユーザーベースの認証」が選択できる

## 相互認証
お互いに証明書を提示しあって認証しあう仕組み（Mutual TLS）

技術的な基盤として**PKI（公開鍵基盤）**が使用される

### 用語整理
| 項目 | 意味 |
| ---- | ---- |
| CA（認証局） | 証明書を発行する信頼できる存在（digicertなど） |
| クライアント証明書 | 各端末に割り当てるX.509証明書`.crt`（秘密鍵`key`とセット） |
| サーバー証明書 |  認証を行うサーバーの証明書 |
| ルート証明書 | CAが発行する証明書 |
| OpenSSL | 証明書や鍵を作成する暗号ツールキット |
| OpenVPN | セキュアなVPNトンネルを作成するソフトウェア |
| easy-rsa | OpenVPNに含まれるOpenSSLを呼び出すための便利スクリプト集 |


VPNは公的な証明書である必要はないので基本的に**自己署名証明書**（オレオレ証明書）を利用


## 全体の流れ

1. 認証局（CA）を自分で作成
2. クライアント証明書を発行（CAで署名）
3. AWSにルートCA、サーバ証明書・秘密鍵を登録
4. クライアント端末に`.ovpn`設定を行う。
5. AWS VPN clientで接続


## 設定
OpenVPNプロジェクトのgitをクローン
```bash
git clone https://github.com/OpenVPN/easy-rsa.git
cd easy-rsa/easyrsa3

# 新しいPKI環境を初期化
./easyrsa init-pki

# 新しいCAを構築
./easyrsa build-ca nopass

# サーバー、クライアントの証明書とキーを作成
./easyrsa --san=DNS:server build-server-full server nopass
./easyrsa build-client-full client1.domain.tld nopass

```
```bash
# フォルダに格納
mkdir ~/custom_folder/
cp pki/ca.crt ~/custom_folder/
cp pki/issued/server.crt ~/custom_folder/
cp pki/private/server.key ~/custom_folder/
cp pki/issued/client1.domain.tld.crt ~/custom_folder
cp pki/private/client1.domain.tld.key ~/custom_folder/
cd ~/custom_folder/

# ACMに各証明書、キーをルートCA証明書でチェーンして登録
aws acm import-certificate --certificate fileb://server.crt --private-key fileb://server.key --certificate-chain fileb://ca.crt

# クライアント証明書は登録する必要ない
aws acm import-certificate --certificate fileb://client1.domain.tld.crt --private-key fileb://client1.domain.tld.key --certificate-chain fileb://ca.crt
```
