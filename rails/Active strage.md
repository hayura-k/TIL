# Active strage

画像をアップするgemは、**carrierwave**や、**Active storage**がある。Rails5.2から、active storageが標準でついているので、Active storage
が使われることが多くなっていくとの事。

### そもそもActive Storageとは
>Active StorageとはAmazon S3、Google Cloud Storage、Microsoft Azure Storageのような クラウドストレージサービスへのファイルのアップロードと
それらのファイルをActive Recordオブジェクトに添付することを容易にします。 
開発およびテスト用のローカルディスクベースのサービスが付属しており、ファイルをバックアップおよび移行用の従属サービスにミラーリングすることができます

説明を聞いてもよくわからなかった。  
簡単に言うなら、クラウドに簡単に画像ファイルをあげることができ、そのファイルデータを簡単にモデルと結びつけることが
できると言うことだ。テーブルのレコードに直接レコードを挿入するのではなく、**別テーブルのファイルデータを入れて、そのテーブル同士を結びつける**と言うものだ。

### 実装の流れ 
1. まずはActive storageをインストールしないといけない。  
```rails active_storage:install```このコマンドで、インストールでき、マイグレーションファイルが作られる。  

実際に作られたマイグレーションフィル
```rb
# This migration comes from active_storage (originally 20170806125915)
class CreateActiveStorageTables < ActiveRecord::Migration[5.2]
  def change
    create_table :active_storage_blobs do |t|
      t.string   :key,        null: false
      t.string   :filename,   null: false
      t.string   :content_type
      t.text     :metadata
      t.bigint   :byte_size,  null: false
      t.string   :checksum,   null: false
      t.datetime :created_at, null: false

      t.index [ :key ], unique: true
    end

    create_table :active_storage_attachments do |t|
      t.string     :name,     null: false
      t.references :record,   null: false, polymorphic: true, index: false
      t.references :blob,     null: false

      t.datetime :created_at, null: false

      t.index [ :record_type, :record_id, :name, :blob_id ], name: "index_active_storage_attachments_uniqueness", unique: true
      t.foreign_key :active_storage_blobs, column: :blob_id
    end
  end
end
```

**active_storage_blobsテーブル**と**active_storage_attachmentsテーブル**の二つのテーブルが作られる。

### active_storage_blobsテーブル・active_storage_attachmentsテーブル　　
- active_storage_blobsには、ファイルの実体を直接保存するわけではなく、ファイル名や、サイズ、ファイルへのpathなどが保存される。  
**ファイルの実態は、development環境ではstorageディレクトリ、production環境ではS3などのサービスに保存される。**
config/storage.ymlに画像ファイルの保存先が記載されてある。

- active_storage_attachmentsテーブルは、active_storage_blobsテーブルと画像を添付したいモデルを結びつける役割をしている。
中間テーブルの役割をしている。

2. **モデルに画像を添付できるようにする。**
```rb
class User < ApplicationRecord
    has_one_attached :image
end
```
has_one_attachedと言うメソッドで、一つのレコードに一つの画像を貼り付けることができる。has_one_attachedの引数の値が（ここでは```:image```）
がname属性の値になる。

これが実装の流れになる。  

フォームを作るときは、```
f.file_field :image```

と書いて、inputタグを作成する。画像をviewに出力するときは、image_tag @user.imageなどとして出力できる。
image.attached?メソッドで画像が添付されているかどうかの判定ができる。

(参考サイト)[https://qiita.com/eightfoursix/items/a47ce1bd945582f5d808]


ちなみに画像はサイズ調整もすることができる。
(参考)[https://qiita.com/kazuomatz/items/3cdbd2c40576c2e9d89b]
**ImageMagic**をインストールすることでサイズ調整のメソッドを使うことができる。
