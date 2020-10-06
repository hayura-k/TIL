# ActiveRecord

ランキング機能を実装する際に、ActivRecordのgroupメソッドやjoinsメソッドが出てきて、具体的にテーブルがどう変化して行っているのかが
わかりづらかったので、調べた。

### まずGROUP BYの確認

group byは単一の情報だけじゃなくて、複数のデータを集計した結果を取得したいときに使用する。

```sql
SELECT users.id, users.name, users.mail, COUNT(posts.id) AS post_number, COUNT(favorites.id) AS favorites_number 
FROM users 
LEFT JOIN posts 
ON users.id = posts.user
LEFT JOIN favorites 
ON users.id = favorites.user
WHERE users.act = 1 
AND posts.act = 1 
GROUP BY users.id 
ORDER BY users.id DESC 
```
SELECT以下のSQL文では、[このような](https://gyazo.com/ad51018eba42156d613f21635e0e40d7)テーブルができるだろう。この場合、COUNTなどの
集計関数を使わず、select posts.idなどと書いたらどのデータを取得していいか分からないため、COUNTなどを使う必要がある。

[MySQL の GROUP BY 完全に理解した \- Qiita](https://qiita.com/Go-Noji/items/5baeb790ade4b57126ff)

### groupメソッドについて

```rb
User.group(:sex)
```
などとしたら、```SELECT users.* FROM users GROUP BY users.sex```のSQL文が発行されて男女ごとのidが一番若いレコードがそれぞれ取り出される。

groupメソッドは、単体で使ってもあまり効果は得られず、countやsumなどとともに使われることで効果を発揮する。また、groupメソッドの返り値はハッシュ形式で帰ってくる。


自分のアプリの場合  
```rb
Like.group(:post_id).order('count_all desc').count
# 返り値
=> {16=>2, 17=>2, 20=>1}
```
このようにすることで、上図のようなハッシュ値が帰ってくる。  
```count_all```は、
```rb 
User.group(:age).count
```
によって発行されたSQL文
```sql 
SELECT COUNT(*) AS count_all, `users`.`age` AS users_age FROM `users` GROUP BY `users`.`age
```
のなかのcount_allから取られている。これは```集計メソッド_カラム名```というような作られ方をしている。  

[【Rails】groupメソッドの使い方を図解形式で仕組みを徹底解説！ \| Pikawaka \- ピカ1わかりやすいプログラミング用語サイト](https://pikawaka.com/rails/group)

明日はjoinsメソッドについて調べる。
