# 【Unity】アプリにプッシュ通知を組み込もう！

![画像1](/readme-img/001.png)

## 概要
* [ニフティクラウドmobile backend](http://mb.cloud.nifty.com/)の『プッシュ通知』機能を実装したサンプルプロジェクトです
* 簡単な操作ですぐに [ニフティクラウドmobile backend](http://mb.cloud.nifty.com/)の機能を体験いただけます★☆

## ニフティクラウドmobile backendって何？？
スマートフォンアプリのバックエンド機能（プッシュ通知・データストア・会員管理・ファイルストア・SNS連携・位置情報検索・スクリプト）が**開発不要**、しかも基本**無料**(注1)で使えるクラウドサービス！

注1：詳しくは[こちら](http://mb.cloud.nifty.com/price.htm)をご覧ください

![画像2](/readme-img/002.png)

## 動作環境の準備
### 共通
* Unityインストール(v.5.3以降)

### Android端末で動作確認をする場合
* PC
* Googleアカウント
* Android端末

### iOS端末で動作確認をする場合
* Mac
 * キーチェーンアクセスを利用します
* Apple Developer Program(有償)アカウント
 * 別のMacで使用しているアカウントの場合、発行する証明書に秘密鍵を紐付けることができません。ただし、アカウントを使用しているMacから秘密鍵を書き出して、今回使用するMacに送ることで作業は可能です
* iOS端末
 * Lightningケーブル（端末のUDIDを調べるために必要です）

※このサンプルアプリは、実機ビルドが必要です

## プッシュ通知の仕組み
* ニフティクラウドmobile backendのプッシュ通知は、各プラットフォームが提供している通知サービスを利用しています
 * Androidの通知サービス __GCM（Google Cloud Messaging）__

 ![画像a1](/readme-img/a001.png)

 * iOSの通知サービス　__APNs（Apple Push Notification Service）__

 ![画像i1](/readme-img/i001.png)

* 上図のように、アプリ（Monaca）・サーバー（ニフティクラウドmobile backend）・通知サービス（GCMあるいはAPNs）の間で認証が必要になります
 * 認証に必要な鍵や証明書の作成は作業手順の「0.プッシュ通知機能使うための準備」で行います

## 作業の手順
### 0.プッシュ通知機能使うための準備
* 動作確認を行う端末に応じて該当する内容を準備してください

#### Android端末で動作確認をする場合
* Google Cloud Platform にログインします　https://console.cloud.google.com/
* GCMを利用するためプロジェクトを作成し、APIキー（認証用の鍵）を発行します

![画像a2](/readme-img/a002.png)

![画像a3](/readme-img/a003.png)

![画像a4](/readme-img/a004.png)

* プロジェクトコードの確認をします

![画像a5](/readme-img/a005.png)

* GCMの有効化の設定をします

![画像a6](/readme-img/a006.png)

* Android端末で動作確認をする場合の準備は以上です

#### iOS端末で動作確認をする場合
* Android端末と違い、作業が少し複雑です
 * 作り方を誤ると動作しない可能性があります
 * 丁寧に作業していきましょう！
* 下図の内容を作成していきます

![画像i2](/readme-img/i002.png)

* 作成したファイルはダウンロードし、同じフォルダにまとめておきましょう

* ①CSRファイルを作成します　※初回利用時のみ
 * __注意__：CSRファイルは既に作成したものがあれば、新しく作成しないでください！必ず既存のものを使用します。複数作成してしまうとファイル名が同じであるため区別できなくなり失敗につながる恐れがあります。
* 「キーチェーンアクセス」を開いて、メニューバーの「キーチェーンアクセス」＞「証明書アシスタント」＞をクリックします

![画像i3](/readme-img/i003.png)

* 「鍵ペア情報」を確認して「続ける」をクリックし、「設定結果」が出るので「完了」をクリックします
* 保存場所を選択して「保存」をクリックします

![画像i4](/readme-img/i004.png)

* ここから[Apple Developer Programのメンバーセンター](https://developer.apple.com/account/)にログインして作業を行います

![画像i5](/readme-img/i005.png)

* ②開発用証明書(.cer)を作成します　※初回利用時のみ
 * __注意__：開発用証明書(.cer)ファイルは既に作成したものがあれば、新しく作成しないでください！必ず既存のものを使用します。複数作成してしまうとファイル名が同じであるため区別できなくなり失敗につながる恐れがあります。
* 「Certificates」＞「All」＞右上の「＋」をクリックして、「iOS App Development」にチェックをいれます

![画像i6](/readme-img/i006.png)
![画像i7](/readme-img/i007.png)
![画像i8](/readme-img/i008.png)

* ③AppIDを作成します
* 「Identifiers」＞「App IDs」＞右上の「＋」をクリックします

![画像i9](/readme-img/i009.png)
![画像i10](/readme-img/i010.png)
![画像i11](/readme-img/i011.png)
![画像i12](/readme-img/i012.png)

* ④動作確認で使用する端末の登録をします
 * 既に登録済みの場合、この作業は不要です
* 「Devices」＞「All」＞右上の「＋」をクリックします

![画像i13](/readme-img/i013.png)

* UDIDは下記のいずれかの方法で調べることができます

![画像i14](/readme-img/i014.png)
![画像i15](/readme-img/i015.png)

* 先ほどの入力欄に調べたUDIDをコピーして貼り付け、「Continue」をクリックします

![画像i16](/readme-img/i016.png)

* ⑤プロビジョニングプロファイルを作成します
* 「Provisioning Profiles」＞「All」＞右上の「＋」をクリックします

![画像i17](/readme-img/i017.png)
![画像i18](/readme-img/i018.png)
![画像i19](/readme-img/i019.png)
![画像i20](/readme-img/i020.png)

* ⑥APNs用証明書(.cer)の作成をします
* 「Certificates」＞「All」＞右上の「＋」をクリックします

![画像i21](/readme-img/i021.png)
![画像i22](/readme-img/i022.png)

* CSRファイルは①開発用証明書(.cer)作成時と同じものを使用します

![画像i23](/readme-img/i023.png)
![画像i24](/readme-img/i024.png)

* 証明書などの作成は以上です
* 次に、作成した証明書から必要なp12形式で証明書を書します
 * ⑦開発用証明書(秘密鍵.p12)
 * ⑧APNs用証明書(.p12)

* ⑦開発用証明書(秘密鍵.p12)を書き出します
* ②で作成した（あるいは既存の）「開発用証明書(.cer)」をダブルクリックしてキーチェーンアクセスを開きます

![画像i25](/readme-img/i025.png)

* 選択した証明書を右クリックします

![画像i26](/readme-img/i026.png)

* ⑧APNs用証明書(.p12)を書き出します
* キーチェーンアクセスを開きます
* ⑥で作成した「APNs用証明書(.cer)」をダブルクリックしてキーチェーンアクセスを開きます
* 三角のアイコンをクリックして、開きます
 * 重要：証明書と秘密鍵を別々にする必要があります！

![画像i27](/readme-img/i027.png)
![画像i28](/readme-img/i028.png)

* iOS端末で動作確認をする場合の準備は以上です

* この後使用する証明書やファイルを確認しておきましょう
 * ②開発用証明書(.cer)・⑤プロビジョニングプロファイル・⑦開発用証明書(秘密鍵.p12)・⑧APNs用証明書(.p12)


### 1. [ニフティクラウドmobile backend](http://mb.cloud.nifty.com/)の会員登録とログイン→アプリ作成と設定
* 上記リンクから会員登録（無料）をします。登録ができたらログインをすると下図のように「アプリの新規作成」画面が出るのでアプリを作成します

![画像3](/readme-img/003.png)

* アプリ作成されると下図のような画面になります
* この２種類のAPIキー（アプリケーションキーとクライアントキー）はXcodeで作成するiOSアプリに[ニフティクラウドmobile backend](http://mb.cloud.nifty.com/)を紐付けるために使用します

![画像4](/readme-img/004.png)

* 続けてプッシュ通知の設定を行います

![画像5](/readme-img/005.png)

### 2. Unityでプロジェクトを開く

* [GitHub](https://github.com/ncmbadmin/unity_push_quickstart)の「Clone or download▼」＞「Download ZIP」をクリックして、プロジェクトをダウンロードします
* ZIPファイルを解凍します
* Unityを起動し、「open」を選択してダウンロードしたプロジェクトを指定します
* 「Start」シーンを開きます

### 3. APIキーの設定

* NCMBSettingsオブジェクトをタブし、inspectorを開きます
* 先程[ニフティクラウドmobile backend](http://mb.cloud.nifty.com/)のダッシュボード上で確認したAPIキーを貼り付けます
* Android端末で動作確認をする場合はプロジェクト番号も貼り付けます
* 書き換え終わったら「保存」をクリックして保存をします

![画像7](/readme-img/007.png)

### 4. 実機ビルド
* 動作確認を行う端末に応じて該当する作業を行ってくださ

#### Android端末で動作確認をする場合
* ★こちらのドキュメントを参考して手順記載お願いします。[ドキュメント](http://mb.cloud.nifty.com/doc/current/push/basic_usage_unity.html#Android%E7%AB%AF%E6%9C%AB%E3%81%B8%E3%81%AE%E3%83%93%E3%83%AB%E3%83%89)

#### iOS端末で動作確認をする場合
* ★こちらのドキュメントそ参考して手順記載おねがいします。[ドキュメント](http://mb.cloud.nifty.com/doc/current/push/basic_usage_unity.html#iOS%E7%AB%AF%E6%9C%AB%E3%81%B8%E3%81%AE%E3%83%93%E3%83%AB%E3%83%89)

### 5.動作確認
* インストールしたアプリを起動します
 * プッシュ通知の許可を求めるアラートが出たら、必ず許可してください！
* 起動されたらこの時点でAndroid端末はレジスタレーションID、iOS端末はデバイストークンが取得されます
* [ニフティクラウドmobile backend](http://mb.cloud.nifty.com/)のダッシュボードで「データストア」＞「installation」クラスを確認してみましょう！

![画像12](/readme-img/012.png)

* 端末側で起動したアプリは一度閉じておきます

### 6.__プッシュ通知を送りましょう！__
* いよいよです！実際にプッシュ通知を送ってみましょう！
* [ニフティクラウドmobile backend](http://mb.cloud.nifty.com/)のダッシュボードで「プッシュ通知」＞「＋新しいプッシュ通知」をクリックします
* プッシュ通知のフォームが開かれます
* 必要な項目を入力してプッシュ通知を作成します

![画像13](/readme-img/013.png)

* 端末を確認しましょう！
* 少し待つとプッシュ通知が届きます！！！

## 解説

サンプルプロジェクトに実装済みの内容のご紹介
★ここに記載、お願いします


## 参考
* ニフティクラウドmobile backend のドキュメントもご活用ください
 * [クイックスタート](http://mb.cloud.nifty.com/doc/current/introduction/quickstart_unity.html)
 * [プッシュ通知](http://mb.cloud.nifty.com/doc/current/push/basic_usage_ios.html)
