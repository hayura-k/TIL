チェリー本のクラスの所を復習した。

# クラスの作成  

クラスは、**オブジェクト指向**の設計で作られるものである。  
例えば、userが名前などの年齢を持っている時にハッシュと配列を使うと次のように処理できる。  
```rb
users = []
users << {first_name: "alice", last_name: "ruby" ,age: 20}
users << {first_name: "bob", last_name: "php",age: 10}

def full_name(user)
  "#{user[:first_name]} #{user[:last_name]}"
end

users.each do |user|
  puts "#{full_name(user)} #{user[:age]}"
end
```
この場合はハッシュでデータを管理しているので、users[0][:age] = 20などで簡単にデータの変更ができてしまう。  
小規模なアプリだとそれでもいいが、大規模なアプリの場合はもっと堅牢に作らないといけない。
また、ハッシュだとデータを読み取る時に、タイプミスをするとnilが帰ってくるためエラーわかりづらい。

クラスを使った場合は次のように書くことができる。  
```rb
class User
  attr_reader :first_name, :last_name, :age
  
  def initialize(first_name, last_name, age)
    @first_name = first_name
    @last_name = last_name
    @age = age
  end
  
  def full_name
    "#{@first_name} #{@last_name}"
  end
end

users = []
users << User.new('Alice', 'Ruby',20)
users << User.new('Hara', 'Python', 10)

users.each do |user|
    puts "#{user.full_name} #{user.age}"
end
```
この場合は、オブジェクトのデータを変更することはできない。（attr_accessorとかなら変更できる。）
タイプミスをしたときもエラーが出るため、タイポに気付きやすい。  

クラスの最大の特徴が、**データとメソッドを同時に保持しているところ**だ。

## 用語の紹介

### クラス  
よく「設計図」に例えられる。rubyのオブジェクトは何かしらのクラスに属している。  

### オブジェクト、インスタンス、レシーバ  
クラスを元にして作られたデータのかたまりを**オブジェクト、またはインスタンス**と言う。
メソッドを呼び出す側として扱うときは、**レシーバ**と言う。  

### メソッド、メッセージ  
オブジェクトの振る舞いを**メソッド**と呼ぶ。javascriptで言うなら「関数」に当たる!。
```user.full_name``` このやり方で、メソッドを呼び出すことができる。  

### 属性  
オブジェクトから取得できる値のことを**属性（アトリビュート、プロパティ）**と言う。
activerecoredでは、この属性とDBのカラムが対応することになる。
