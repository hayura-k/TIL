# beforeとletの違いについて

before、letはどちらもファクトリを元にして、テストデータを作成するメソッドだが、作り方や使い方に違いがあったのでまとめてみた。

### before
beforeを使ったテストデータの作り方
```rb
before do
    @user = User.create(
      first_name: "Joe",
      last_name: "Tester",
      email: "joetester@example.com",
      password: "dottle-nouveau-pavilion-tights-furze",
    )

    @project = @user.projects.create(
      name: "Test Project",
    )
end
```
このようにインスタンス変数に代入して使う。ブロック外からもこの変数を利用するためにインスタンス変数にしてしおうという考えだ。
また、毎回beforeは呼ばれるため、テストデータも毎回作られる。


### let
letを使ったテストデータの作り方
```rb
let(:user) { FactoryBot.create(:user)}
let(:project) { FactoryBot.create(:project, owner: user)}
```
これはブロックで囲まれていないので、インスタンス変数にする必要がない。また、シンボルで書くためインスタン変数ではかけない。  
シンボルの値を変数のように扱い、値を呼ぶときは```user,project```のようにするだけで変数と同じように呼び出せる。
最大の違いが、letは**遅延評価と呼ばれるところだ。** 値が呼ばれるまでは、データが作られない。

### letで問題が発生する場面
```rb
let(:note1) { 
   FactoryBot.create(:note,
      project: project,
      user: user,
      message: "This is the first note"
   )
}

#一致するデータが一件も見つからない時
context "when no match is found" do
  it "returns an empty collection" do
     expect(Note.search("message")).to be_empty
     expect(Note.count).to eq 1 
  end
end
```
このような場合は、context内でnote1が呼び出されていないため、データベースにまだデータが入っておらずcountしてもエラーになる。

そのような場合は。```let!```を使わないといけない。letを使う場合とbeforeを使う場合は考えて使い分けないといけない。無理にlet!を使う必要はない。

### 振り返り
letは現場railsでも引っかかった気がして、今日やっと腹落ちした感じがしたのでよかった！




