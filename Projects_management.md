# プロジェクト管理方法


## 目的

個人開発プロジェクトの管理方法の手順についてつづっていく。
何かと乱立しやすいので

---

## 各ステップ

| ステップ | 内容 | ツール例
| ----- | ----- | ----- |
| ①要件定義 | 何を作るかを言語化 | Markdown, Zenn |
| ②設計（UI・機能） | 動きや画面をスケッチ | drow.io, Figma |
| ③設計（システム） | ファイル構成や処理の流れを図解 | PlantUML, 図+README |
| ④実装 | 仮想環境・バージョン管理で整理し進める | venv, Git |
| ⑤テスト | 試して問題ないか確認 | 手動 or python |
| ⑥ドキュメント整備 | READMEに構成・使い方を書く | Github, README |

---

## フォルダ構成

**フォルダ構成**
```
F:\Projects
└── Project01
└── Project02
    ├── .env\             # 仮想環境（Git無視）
    ├── src\              # ソースコード（Pythonなら.pyファイル群）
    │    └── main.py
    ├── docs\             # ドキュメント類（仕様書・設計書）
    ├── tests\            # テストコード（unittestなど）
    ├── .gitignore        # 無視するファイルリスト
    ├── requirements.txt  # パッケージ管理ファイル
    ├── README.txt        # プロジェクト説明書
    └── setup.bat         # 仮想環境構築やセットアップ用スクリプト（任意）
```

---

仮想環境は必要なら作成
**プロジェクトルートにて仮想環境の構築**
```python
python -m venv .env   #隠しファイル
```

**ライブラリインストール**
```python
pip install xxx
```

**パッケージ一覧をrequirementsに抽出**
```python
pip freeze > requirements.txt
```

**コード作成後にsrcフォルダ内のコードとrequirementsをsite-packagesにコピー**
```powershell
cd F:\Projects\xxxx
copy .env\requirements.txt .\docs
copy .\src\*.py .\.env\Lib\site-packages\
```

**compress.ps1でzip化**
```ps1:compress

$SevenZIP = "C:\Program Files\7-zip\7z.exe"

# 圧縮対象フォルダ指定
$Source = Join-Path $PSScriptRoot ".env\Lib\site-packages"

# 宛先指定
$Pname = Split-Path $PSScriptRoot -Leaf
$Dest = "$Pname.zip"


# zipフォルダが存在したら削除
if (Test-Path $Dest) {
    Remove-Item $Dest -Force
}

# 圧縮実行
& $SevenZIP a -tzip $Dest "$Source\*"
```

