# Gitの使い方
Gitはファイルの変更履歴を管理する仕組み
リポジトリという容器にファイルを格納しバージョン管理を行う

<br>

## 基本用語
①リポジトリ
・ローカルリポジトリ：ローカル端末にあるリポジトリ
・リモートリポジトリ：githubなどのインターネット上のリポジトリ

②変更操作
・add：リポジトリに変更を記録する前にステージにまとめる
・commit：リポジトリに変更を記録
・push：リモートリポジトリにアップロード

<br>

## 基本的な流れ

作業ディレクトリ （F:\Projects\Project01）
↓
ステージングエリア（git add）
↓
ローカルリポジトリ（git commit）
↓
リモートリポジトリ（git push）
という流れで変更を記録。


<br>


## リモートリポジトリとの連携
### ssh接続の設定
ホームディレクトリに.sshを作成し、鍵の作成
```
ssh-keygen -t rsa
```

ssh鍵のファイル名をデフォルトから変更した場合、.ssh/configの設定も変更する
＊デフォルトだと読みに行くファイル名が違うため
```
Host github github.com
  HostName github.com
  IdentityFile ~/.ssh/id_git_rsa #ここに自分の鍵のファイル名
  User git
```

ssh接続確認
```
ssh -T github
```



<br>

## 初期設定

ローカルのGitにGitHubのユーザーメイとメールアドレスを設定
```
git config --global user.name 'user_name' # user_nameにはGitHubに登録したuser nameを入力

git config --global user.email 'user@gmail.com' # user@gmail.comにはGitHubに登録したメールアドレス
```
ホームディレクトリ直下の.gitconfigに記録される

<br>



<br>

## 基本コマンド
**gitリポジトリの作成**
```git
cd /d F:\Projects\Projectxx
git init
```
基本的にプロジェクトごとに用意する

<br>

**ステージング、コミット**
リポジトリに反映（保存）
```git
git add ファイル名（or フォルダ名）
git add .　（カレントディレクトリの全て）

git commit -m "コメント"
```
<br>

リモートリポジトリの設定
git initごとにリモートリポジトリを設定する
```git
git remote add origin git@github.com:[github_username]/[remoterepositoryname].git
```
originというショートカットにリモートリポジトリのURLを登録

<br>

**リモートリポジトリにpush**
pushするブランチを指定してoriginにpush
```
git push -u origin main
```


---

## ブランチについて
ブランチとは違うバタフライエフェクトの異なる世界線のイメージ
メインブランチ、開発ブランチ、修正ブランチなど世界線を分けることでメインに影響することなく機能回収やバグフィックスを行うのが目的
世界線を合成することをマージという

**ブランチ作成**
```
git branch new_branch
git branch -a  #ブランチ確認
```

**ブランチ切り替え**
```
git checkout new_branch
```

**ブランチ名変更**
``` 
git branch -m master main
```


**マージ**
```
git checkout main
git merge new_branch
```

## 修正
gitのリセットは.gitフォルダを削除すればよい
```
rm -rf .git
```
ステージングの取り消し
```
git reset
```

## リモートリポジトリのマージ
リモートリポジトリ（Github）のマージにはpull requestが必要
pull requestを確認することでリモートリポジトリ上のマージが行える。
 

## Git作業フロー
developブランチで開発を行い、mainブランチにマージするのが基本
それぞれのブランチもGithubにpushする。
mainにマージする前、developで開発を始める前にpullで最新状態にする
必ずmain以外をコミットしてからpull requestでマージしましょう
```git
# 通常作業
git checkout develop
git add .
git commit -m "comment"
git push -u origin develop

# リリース（mainへ反映）
git checkout main
git pull origin main
git merge develop
git push -u origin main

# developを最新化
git checkout develop
git merge main
git push -u origin develop
```
<br>

## 別端末からリモートリポジトリに接続
### リポジトリをクローン
```git
git clone git@github.com:xxxx/xxxx.git
```
これを実行するとgithub上のデフォルトのブランチがクローンされる。

### featureブランチを作成、クローンし切り替える
```git
git checkout -b feature origin/feature
```
`-b`は「新しいブランチを作成する」
`checkout`で自動切り替え