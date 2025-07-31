## Dockerの基本

パッケージされた１つの計算環境のことを**イメージ**とよぶ。
イメージはDocker Hubなどのリポジトリからダウンロードするか、自分で作成する

イメージを作成するためのレシピを記述したファイルを**Dockerfile**

起動状態にある計算環境のことを**コンテナ（Container）**

![alt text](Dockerimage.png)

<br>

情報取得
```
# dockerイメージ一覧
docker images

# dockerコンテナ一覧
docker ps -a
```

<br>

コンテナ操作
```
# コンテナ起動、ログイン（新規）
docker run -it labc_v2

# バックグラウンド起動
docker run -d labc_v2

# コンテナ起動,再起動
docker start labc_v2

# コンテナ削除
docker rm labc_v2

# 起動中のコンテナのbashを起動
docker exec -it container bash
```