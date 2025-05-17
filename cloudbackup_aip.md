# クラウドバックアップ戦略 ActiveImageProtecter2022（AIP）
AIPをもちいてクラウド（Wasabi Hot Cloud Strage）にファイルサーバーをバックアップする手法について綴る。

## タスクスケジュールにて管理
大容量の場合、クラウドにアップロードするのに1日では終わらないケースがある。
クリアするには毎日少しずつアップロードしていく必要がある。
AIPをインストールするとC:\Program files\Actiphy\ActiveImage Protectorにaipcontrol.exeというソフトがあり、これをbatにて処理することでタスクの一時停止と再開を行うことができる。


**タスク一時停止**
```bat:task_pause
@echo off

cd "C:\Program files\Actiphy\ActiveImage Protector
aipcontrol task xxxx pause
```

**タスクの再開**
```bat:task_resume
@echo off

cd "C:\Program files\Actiphy\ActiveImage Protector
aipcontrol task xxxx resume
```
