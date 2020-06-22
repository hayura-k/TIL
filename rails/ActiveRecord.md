# ActiveRecord
みけたさんがactiverecordについて、良い記事を上げていてくれたので、自分でも学習してみた。

- ActiveRecord自体はrubyの外部ライブラリである。ORマッパーと呼ばれ、rubyを用いてDBのSQLを発効することができる。

- Railsでは、このActiverecordをApplicationRecordが継承しているので、rubyのコマンドでオブジェクトを作成することができる。
```rb
class ApplicationRecord < ActiveRecord::Base
  self.abstract_class = true
end
```
**Activerecordを通してよく使うメソッド**(参考)[https://qiita.com/ryokky59/items/a1d0b4e86bacbd7ef6e8]

### attr_accessor

ここからが今日一番勉強になった。Activerecordを継承しているから、```User.find(1)```などのコマンドを使えるというのはわかっていたがもっと深い理由までは
わかっていなかった。

#### 前提
```rb
class User
  def initialize(name,age) 
    @name = name
    @age = age
  end
end

user.new("yuki",22)
user.name  #undefiened method `name`となるだろう。
user.name = "hoge" #undefined method `name=`と出てくるだろう。
```
このように**クラスの外からオブジェクトの値を取り出すことはできない。**

ではなぜ取り出すことができているのだろうか？  
それは**attr_accessorメソッド**のおかげである。attr_accessorメソッドは**「外部からインスタンス変数の値を読み書きするメソッド」**である。
これにより、インスタンス作成時にできたインスタンス変数にアクセスすることができ、  
```rb
user.name #=> "yuki"
user.name = "hoge" 
user.name #=> "hoge"
```
となるだろう。そして、attr_accessorメソッドがActiveRecord::Baseクラスに記載されてあるから、それを継承してるクラスのオブジェクトでもそう書くことができる。

(参考)[https://qiita.com/okaeri_ryoma/items/733761ca46f2eadf8ae7]

改めてしっかりと勉強できた。最近チェリー本を読んだありがたみがだんだんとわかってきた。チェリー本はバイブルにしたい。
