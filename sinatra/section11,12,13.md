### リレーションについて  

1対多の関係を示す。親子関係ともいう。親のほうのモデルに***has_many :todos***,子のほうのモデルに***belongs_to :category*** などと書いておくことで、  
勝手にテーブルのリレーションができる。また、そのことによって***todo.category.name*** というメソッドでcategoryの名前を呼び出すことができる。

親子関係を作るためには、外部キーがないといけないのでマイグレーションにcategory_idなどの参照する予定のIDのカラムをあらかじめ追加しておく。

### ページネーション  
一つのページに決まった数のデータしか入らないようにする仕組み。  
gem の kaminariを使用する。Githubのkaminariを見て仕様した。  
views側では<pre><%= paginate @users %></pre>を書き込む。  
paganateが対象にしているインスタンス変数は、<pre>@users = User.order(:name).page params[:page]</pre>で定義する。

1ページに採るデータはconfigから設定する。ほかにもやり方はあると思うが、とりあえずUdemyからはこのページネーションの表示の仕方を学んだ。

### リンクによる値の更新  
<pre>resources :todos do
     member do
      patch 'status'
     end
end</pre>このmember do ~ endによってtodo_path(todo)の先頭にstatusを追加してルーティングにstatus_todo_path(todo)を追加することができる。  
#### 一番驚いたこと  
link_toヘルパーでもパラメーターを送ることができる！！  
<pre>link_to '完了', status_todo_path(todo, value: 1)</pre>
受け取るときはparams[:value]で受け取れる。わからなくなった検証ツールで調べればいい。
