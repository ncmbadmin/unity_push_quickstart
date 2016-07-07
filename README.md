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
* 開いたプロジェクトの【ヒエラルキー(Hierarchy)ビュー】から、NCMBSettings、NCMBManagerオブジェクトを確認します
  * (※ 存在しない場合は、【ヒエラルキー(Hierarchy)ビュー】からCreate Emptyを作成し、各オブジェクト名称に更新後、【プロジェクト(Project)ビュー】から各ソースをドラッグ＆ドロップします）

![画像6](/readme-img/006.png)

### 3. APIキーの設定

* NCMBSettingsオブジェクトをタブし、inspectorを開きます
* 先程[ニフティクラウドmobile backend](http://mb.cloud.nifty.com/)のダッシュボード上で確認したAPIキーを貼り付けます
* 「Use Push」にチェックを入れます
* Android端末で動作確認をする場合はプロジェクト番号を貼り付けます

![画像7](/readme-img/007.png)

### 4. 実機ビルド
* 動作確認を行う端末に応じて該当する作業を行ってください

#### Android端末で動作確認をする場合
* Android端末向けにビルドを実行する事で.apkファイルを作成する必要があります
* Android端末にプッシュ通知を行う場合は、Android Manifestの設定が必要になります
  * `/Assets/Plugins/Android/AndroidManifest.xml`では、以下の__パッケージ名__に注意して設定する必要があります
  * __パッケージ名__を正しく設定します(Bundle Identifierと一致為)
  * __パッケージ名__(例：com.nifty.push.quickstart)を__YOUR_PACKAGE_NAME__の文字列で__一括置換__すると便利です

```xml
 1) パッケージ名が正しく設定されているか確認する【YOUR_PACKAGE_NAME】

<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"

    package="YOUR_PACKAGE_NAME" >
```
```xml
2)パッケージ名.permission.C2D_MESSAGEという書き方になっているか確認する【YOUR_PACKAGE_NAME】

    <!-- Put your package name here. -->
    <permission android:name="YOUR_PACKAGE_NAME.permission.C2D_MESSAGE"
        android:protectionLevel="signature" />

    <!-- Put your package name here. -->
    <uses-permission android:name="YOUR_PACKAGE_NAME.permission.C2D_MESSAGE" />
```
```xml
3)通常はcom.nifty.cloud.mb.ncmbgcmplugin.アクティビティ名を設定する
  Prime31プラグインを利用している場合はcom.nifty.cloud.mb.ncmbgcmpluginをcom.prime31に変更する

<activity android:name="com.nifty.cloud.mb.ncmbgcmplugin.UnityPlayerProxyActivity"
        android:label="@string/app_name"
        android:configChanges="fontScale|keyboard|keyboardHidden|locale|mnc|mcc|navigation|orientation|screenLayout|screenSize|smallestScreenSize|uiMode|touchscreen">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>

```
```xml
4) レシーバータグの中にあるcategoryタグの名前を確認する【YOUR_PACKAGE_NAME】

    <!-- [START gcm_receiver] -->
    <receiver
        android:name="com.nifty.cloud.mb.ncmbgcmplugin.NCMBGcmReceiver"
        android:exported="true"
        android:permission="com.google.android.c2dm.permission.SEND" >
        <intent-filter>
            <action android:name="com.google.android.c2dm.intent.RECEIVE" />
            <!-- Put your package name here. -->
            <category android:name="YOUR_PACKAGE_NAME" />
            <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
        </intent-filter>
    </receiver>
    <!-- [END gcm_receiver] -->
```
* Android端末向けのビルドは、まずメニューバーのFileからBuild Settingsを開きます
  * Platform欄から【Android】を選択し、「Switch Platform」ボータンを押します
  * 「Player Settings...」ボータンを押します
  * 【インスペクター(Inspector)ビュー】から、[Bundle Identifier]には__パッケージ名__と一致するようにします
  * 【インスペクター(Inspector)ビュー】から、Android実機に書き出しエラー[INSTALL_FAILED_CONTAINER_ERROR]の場合為、[Install Location]には__Automatic__に変更します
  * 「Build」ボータンを押します
    * ビルドのapkファイル名と出力場所をダイアログから指定します

![画像a007](/readme-img/a007.png)

![画像a008](/readme-img/a008.png)

* ビルド完了の.apkファイルを確認し、Android実機にインストールします

#### iOS端末で動作確認をする場合

* iOS端末でビルドを行うには、Unityで.xcodeprojファイルを作成する必要があります
* iOS端末向けのビルドは、まずメニューバーのFileからBuild Settingsを開きます
* PlatformにiOSを選択した状態でビルドを実行することで、「.xcodeproj」ファイルが生成されXcodeが起動します

![画像i032](/readme-img/i032.png)

###### Xcodeでビルドを実行する
* Bundle Identifierの設定
TARGETS → Unity-iPhone → General → ▼identity  
Bundle Identifier にApple Developer ProgramのAppIDで設定した、Bundle IDを入力する  
（※必ず同じBundle IDを設定してください）

![画像i033](/readme-img/i033.png)

* Code Signing IdentityとProvisioning Profileを設定する<br>
TARGETS → Unity-iPhone → Build Settings → ▼Code Signing<br>
 * ▼Code Signing Identity → Debug → Any iOS SDK にApple Developer Programで登録した開発用証明書を設定<br>
 * Provisioning Profile にApple Developer Programで作成したProvisioning Profileを設定

![画像i034](/readme-img/i034.png)

設定後に実行することで、iOS端末でのデバッグが可能になります

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

### SDKのインポートと初期設定

    ニフティクラウドmobile backend の[ドキュメント（クイックスタート）](http://mb.cloud.nifty.com/doc/current/push/basic_usage_unity.html)をご用意していますので、ご活用ください

### プッシュ通知プラグインについて

    UnitySDKではプッシュ通知を利用するためのAndroid/iOSプラグインが入っております。
    NCMBSettingsでの「Use Push」チェックすることで、プッシュ通知機能をすぐ利用可能になります。

## 参考
* ニフティクラウドmobile backend のドキュメントもご活用ください
 * [クイックスタート](http://mb.cloud.nifty.com/doc/current/introduction/quickstart_unity.html)
 * [プッシュ通知](http://mb.cloud.nifty.com/doc/current/push/basic_usage_ios.html)
