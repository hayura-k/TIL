# Ransackについて 

ransackとは検索機能をつける時に、使用するGemである。
**使い方の流れ**
1. Gemfileに書き込む。これによる、モデルにransackメソッドが追加される。
2 コントローラーで検索結果のパラメーターを受け取る。  
controller/task_controller.rb
```rb
def index
  @q = Task.ransack(params[:q])
  @tasks = @q.result(distinct: true)
end
```
検索結果を@qに保存し、resultメソッドでオブジェクトを返す。distinctは重複がないように返している。  
@qはqueryを訳してそうなっている。  

3. フォームの設置  
ransackの提供するsearch_form_forヘルパーを使用する。  
```rb
= search_form_for @q do |f|
  .form-group.row
        = f.label :name_cont, '名称', class: 'col-sm-2 col-form-label'
        .col-sm-10
            =f.search_field :name_cont, class: 'form-control'
  .form-group
        =f.submit class: 'btn btn-outline-primary'
```
search_fieldにより、```<input name=q[:name_cont]>```が生成されるため、コントローラーでparams[:q]で受け取る。  
params[:q]としているのは、検索条件を一括で受け取るためだ。

**_cont**によって、DBのLIKE検索が行われている。他にも様々な検索を行うための検索マッチャーがある。　
|検索方法| 英語| 意味|
|------|-----|----|
|_eq| equal| 等しい|
|_not_eq|not equal | 等しくない|
|_lt| less than| より小さい|
|_gteq|greater than or equal |より大きい（等しいものも含む） |
|_cont |contains value| 値を含む|

これらの他にもたくさんあるので、必要な時は随時調べる
