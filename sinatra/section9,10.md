# POSTパラメータについて  

### ストロングパラメータ
<pre>def todo_params
  permit.require(:todo).permit(:name, :body, :status)
end</pre>  
このメソッドによって、{:todo {name: "なまえ", body: "中身", status: "進行度"} } というハッシュ形式で送られてくるデータを受け取ることができる。  

この形式で送られてくるのは、form_withで作られるinputタグが
<pre>input type="text" name="todo[:name]"</pre>
になっているから。name属性に注目。

また、ストロングパラメーターの中に記載されている値以外は受け取ることができないため、それ以外の値を受け取る場合はpermitの中に書き加えないといけない。  

### コールバック  
コールバックは指定したアクションが実行される前に実行されるメソッド。  
<pre>before_action :set_todo, only: [:show,:edit, :destroy, :update] 
  private
  def set_todo
    処理
  end</pre>
この場合はshow,edit,destory, updateの前にメソッドが実行される。
