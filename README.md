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
画像の赤枠で囲ったURLが外部からこのPBLサーバーへ接続する入り口のURLとなるので後に説明するスキルの作成でエンドポイントにこのURLを設定することになる。
なおngrokを終了して再度実行するたびにこのURLは変更されるので注意する。

## 2.Amazon Alexaへの実装
Alexaを通して自分の作った対話システムと会話するにはスキルと呼ばれるALexa用のサービスとして対話システムを作る必要がある。  
AlexaDepveloper Consoleに登録してスキルの作成を行う。  

### 2.1 Amazon Developer Consoleに登録

https://developer.amazon.com/ja-JP/alexa?&&
にアクセスして右上の「ログイン」からログイン画面に行く。Amazonのアカウントを持っていない人はアカウントを作成してログインする。  
ログインできたら右上の３点のマークからAlexa Developer Consoleへ移動する。
この際Amazon開発者ポータルへの登録というページが表示される場合がある。その場合は、ページの指示に従って氏名や連絡先を入力して進む。

### 2.2 スキルの作成
![amazon-skill](https://user-images.githubusercontent.com/108092676/206988066-372d1639-f786-4791-bb57-a7a67106c5a6.png)
Alexa Developer Consoleに移動したら上の画像の赤枠のボタンから新規スキルを作成する。
![amazon-name](https://user-images.githubusercontent.com/108092676/206989060-bf07ec38-a44d-40f4-9416-c68a64b2e8e2.png)
まずはスキルの名前とプライマルロケールを設定する。名前は自由で、プライマルロケールは日本語を選択して次へ進む。

次の画面では３つ選択があるが、以下のように選択する。
### 1. エクスペリエンスのタイプを選択します
その他
### 2. モデルを選択する
カスタム
### 3. ホスティングサービス
独自のプロビジョニング  
選択したら次へ進む。

次の画面では「スクラッチで作成」を選択して次へ進む。  
最後に確認画面が表示され、よければ右上のcreate skill をクリックしてスキルを作成する。

### 2.3 スキルの設定
次にAlexaへの入力を対話システムに送信できるようにスキルのスロット、インテント、エンドポイントの設定を行う。

### 呼び出し名の設定
---
最初にスキルを呼び出すときの名前を設定する。
スキルの呼び出し名は  「アレクサ、○○を起動して」、の○○にあたる部分である。  
画面左のメニューからスキルの呼び出し名をクリックして設定画面を開く。
![image](https://user-images.githubusercontent.com/108092676/207008710-7042a1b7-a265-4629-98d7-08dbef6a4b24.png)

### スロットの作成
---
#### 1. +スロットタイプ　をクリック
![amazon-slot](https://user-images.githubusercontent.com/108092676/206994317-7e534b2d-fe21-405b-ae69-98e3c5089b78.png)

#### 2. Alexaのビルトインライブラリから既存のスロットタイプを使用　を選択し、リストタイプからAmazon.CityとAmazon.Colorを追加する。
![amazon-slotbuiltlit](https://user-images.githubusercontent.com/108092676/206997374-9ab7d9b2-97d0-4546-b80b-82368015a3d3.png)

#### 3. 値を持つカスタムスロットタイプを作成　を選択し、「any_text」と入力して次へをクリック
![amazon-slotname](https://user-images.githubusercontent.com/108092676/206994969-6179d833-7629-4bb3-bfe3-f0e7519e3be5.png)

#### 4. スロット値に適当な日本語(例:ほげほげ)を入れ、+をクリック
![amazon-slotvalue](https://user-images.githubusercontent.com/108092676/206996038-0c74abbc-08ee-400f-86a6-f7f0bc63c85b.png)

### インテントの作成
---
#### 1. +インテントを追加　をクリック
![image](https://user-images.githubusercontent.com/108092676/206999756-1561a902-7fa9-4e82-a2b8-060dac85bfc5.png)

#### 2. カスタムインテントを作成を選択し、名前を「HelloIntent」に設定してカスタムインテントを作成をクリック
![image](https://user-images.githubusercontent.com/108092676/206999979-9afaee5d-8bbf-4023-9bae-e86063209599.png)

#### 3. ページ下部のインテントスロットでany_text_a、any_text_b、any_text_cというインテントスロットを作成し、スロットタイプをany_text、AMAZON.Color、AMAZON.Cityに設定する。
![image](https://user-images.githubusercontent.com/108092676/207001532-85130b96-9db6-46cf-9b3a-5106538c9668.png)

#### 4. サンプル発話に{any_text_a},{any_text_b},{any_text_c}の発話を追加する。
![image](https://user-images.githubusercontent.com/108092676/207002335-647d7104-7866-4455-9fed-1636c126d484.png)

### エンドポイントの設定
---
最後にエンドポイントを設定する。画面左のメニューからエンドポイントを選択する。
![image](https://user-images.githubusercontent.com/108092676/207005081-dda074d2-1b84-47b7-b12a-b965dac81cf0.png)
HTTPSを選択し、デフォルトの地域の欄に起動しているngrokの転送URLを入力し、プルダウンメニューは「開発中のエンドポイントは、証明機関が発行したワイルドカード証明書をもつドメインのサブドメインです」にする。転送URLは1.4.ngrokの動作確認で示した画像の赤枠で囲われた部分である。  
入力したら上の方にある「エンドポイントを保存」をクリックする。
最後に画面左のメニューからスロットタイプを開き、上の方にある「モデルを保存」をクリックした後、「モデルをビルド」をクリックしてモデルのビルドを開始する。エンドポイントのURLを変更したときなどは、保存してモデルのビルドをしないと変更が反映されない点に注意。

## 3.プログラムの改良
ここからはAlexa Developer Consoleからngrokを経由してエンドポイントに送られた情報を受けとって応答を生成できるようにdsbook内のプログラムを改良していく。  

まずは以下のコマンドを実行して必要なライブラリをインストールする。
```
pip3 install flask-ask==0.9.7 pyOpenSSL==17.0.0 Werkzeug==0.11.15 itsdangerous==2.0.1 'cryptography <2.2' MarkupSafe==1.1.0 Jinja2==2.11.3
```
まずはalexa_bot.pyを編集する。ここではvimを使ってファイルを編集する。
```
vim alexa_bot.py
```
を入力するとvimを使ってalexa_bot.pyが開かれる。



ここでは接続がうまくいっているか簡単にテストするために受け取った入力をそのまま返す echo_system.py を使用する。  
