# active storageの本番環境の設定

開発環境では、storageディレクトリに画像が保存されているが、本番環境では、S3などのクラウドサービスに画像ファイルをアップロードしないといけない。

### 実装の流れ

**1.AWSでS3バケットを作成する。**  
やり方はudemyや記事をみて実装した。

**2.IAMユーザー作成**  
作成したあと、access key IDとsecret access keyの二つをメモしておく。csvファイルとして出力できるので出力しておく。

**3.IAMユーザーの認証情報をrails側で設定する**  
awsの認証情報をrails側で設定するときは、**credential**という機能を利用する。
```
aws:
  access_key_id: 先ほど控えたもの
  secret_access_key: 先ほど控えたもの
```
先ほどcsvファイルとして出力したものを、ここに記述する。

### credentialとは？
Rails5.2から追加されたcredentialという機能は、重要な秘密情報を保存するのに役立つ。awsのアクセスキーなどの重要な情報は、**config/credentials.yml.enc**
にハッシュ化して保存されている。このファイルを直接いじるときは、``EDITOR=vi rails credentials:edit```のコマンドでファイルの変更を行うことができる。
このファイルの中身を復元するには、master.keyが必要となるが、master.keyはレポジトリ外に置かれるため、環境変数にmaster.keyを直接保存する必要がある。
**画像を投稿したときにS3に保存するため、その時にmaster.keyから秘密情報を復元すると思われる**

**4. confin/storage.ymlにawsの情報を設定する**  
regionとbucketは、作成したバケット名と地域をかく。ちなみにアジアパシフィック東京は、**ap-northeast-1**となる。  
ここでの、```access_key_id: <%= Rails.application.credentials.dig(:aws, :access_key_id) %>```はcredentials.yml.encのに設定したaccess_key_id
のことである。

**5.config/environment/production.rbを変更する**  
```config.active_storage.service = :local```となっているので、保存先をlocalから先ほどstorageで定義したamazonに変更する。

**6.gemをインストールする。**  
```
gem "aws-sdk-s3", require: false
gem "mini_magick"
```
この二つのgemをインストールする。mini_magickは画像のリサイズする時などに必要なのでインストールしておく。

**7.環境変数にmaster.keyをセットする。**  
master.keyは、.gitignoreに記載されているためgitのリポジトリ外にあるため直接、環境変数にmaster.keyの中身をセットしないといけない。
```
heroku config:set RAILS_MASTER_KEY=`cat config/master.key`
```
RAILS_MASTER_KEYの値は直接、master.keyの値を打ち込んでもいい。

以上が主な流れとなる。credentialの理解や、active storageなどの理解が曖昧だったためかなりてこずった。特にcredentialはよく分からず飛ばした所だったので
かなり勉強になった。アウトプットで、デプロイまでしてよかった。


(参考記事)[https://qiita.com/daichi41/items/af2a56ea46c13ca55fd3]
[https://yukitoku-sw.hatenablog.com/entry/2020/02/04/145331]
