# AlexaとPBLサーバーをつなぐ手順
## 前準備


https://github.com/hirokiyamauch/PBL_dialog

↑この手順の 1.環境作成まで行った状態で以下の手順を行う。

## 1.ngrokのインストール
### 1.1ngrokのユーザ登録
ngrokのURLにアクセスして「sign up for free」のボタンからユーザ登録を行う　→　https://ngrok.com

### 1.2ngrokのファイルダウンロード
今回は前準備でダウンロードしたdsbook内にngrokをインストールする。  
登録が完了したら、PBLサーバーに接続し、dsbookフォルダに移動する。
dsbookフォルダに移動したら以下のコマンドを実行してngrokのファイルをダウンロードする。

```
wget https://bin.equinox.io/c/bNyj1mQVY4c/ngrok-v3-stable-linux-amd64.tgz
```

ダウンロードが完了したら以下のコマンドを実行してファイルを展開する。

```
tar -xzvf ngrok-v3-stable-linux-amd64.tgz
```

### 1.3 ダウンロードしたngrokとアカウントの連携
ユーザ登録を行ったngrokのページに戻り、左のタブから「Your Authtoken」に移動する。
下の画像の赤線を引いた部分に表示されているコマンドをコピーしてngrokをダウンロードしたdsbook内で実行する。

![ngrok-auth](https://user-images.githubusercontent.com/108092676/206976535-8bc6f15e-bb75-40c8-bee9-17cf8e453704.png)

### 1.4.ngrokの動作確認
以下のコマンドを実行するとngrokが実行され、画像のような画面になる。

```
./ngrok http 8080
```
![ngrok-zikkou](https://user-images.githubusercontent.com/108092676/206982037-06ada836-abed-4195-9f07-a9865ed95fd9.png)  
画像の赤枠で囲ったURLが外部からこのPBLサーバーへ接続する入り口のURLとなる。  

## 2.Amazon Alexaへの実装
Alexaを通して自分の作った対話システムと会話するにはスキルと呼ばれるALexa用のサービスとして対話システムを作る必要がある。  
AlexaDepveloper Consoleに登録してスキルの作成を行う。  
https://developer.amazon.com/ja-JP/alexa?&&
