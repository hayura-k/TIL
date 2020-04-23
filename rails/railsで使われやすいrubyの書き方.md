# Ruby  

### ローカル変数とインスタンス変数の違い  
- ローカル変数  
その場限りの一時的な変数。メソッド内で定義したローカル変数は***そのメソッド内でしか使えない。***  
- インスタンス変数  
オブジェクトの持つ変数。オブジェクトのどのメソッドからも参照できる。

### if文とunless文  
- if文は返り値を変数に代入することができる。処理が1行で済む処理は後置ifがよく使われる。  

### メソッドの引数にデフォルト値を設定することができる。  
<pre>def name(full = true, with_age = true)
end</pre>
これにより何も設定しない場合はfullとwith_ageはtrueとなる。ただ、こういう設定をすると分からなくなるので***キーワード引数***でデフォルト値を設定しておく。  
<pre>def name(full:true, with_age: true)
end</pre>

#### 便利な書き方  
- nilガード
number || = 10  これは、number || (number=10) と一緒でnumberに値が入ってないならnumberに10を代入して返すというものである。  

- ボッチ演算子(&.)  
object = nil, object&.name  これにより変数に指定のメソッドが入ってなくてもnilが入っている場合はnilを返してくれる。  
ボッチ演算子を利用するとif文や、三項演算子を書くよりも簡潔に書くことができる。  
<pre>name = if object
  object.name
else
  nil
end</pre>
三項演算子
<pre>name = object ? object.name:nil</pre>
ぼっち演算子
<pre>name = object&.name</pre>

- %記法  
ary = %w(apple orange banana) => ['apple',''orange','banana']  
%Wで配列を作ってくれる。%iの場合はシンボルで配列を作ってくれる。 

- 配列の各要素から特定の値だけ取り出す  
配列の値から要素を取り出すときは、ふつうはeachを使う。  
<pre>name = []
users.each {|user| name << user.name}</pre>
これが普通だが、mapメソッドを使ってもできる。
<pre>names = users.map{|user| user.name }</pre>
もっと短くできる。  
<pre>names = users.map(&:name)</pre>
これら3つの書き方は全部names変数にuserのnameを代入することができる。
