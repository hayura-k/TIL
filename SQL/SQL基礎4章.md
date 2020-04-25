### 検索結果の加工（select文のみ）

絞り込み(where句)はselect,update,delete文で使うことができるが、検索結果の加工だけはselect文だけにしか修飾することができない。  
加工は、一番最後の段階で行われる。  

### 検索結果の主要なキーワード  
- DISTINCT 検索結果から重複行を除外する。  
データの種類を見たいときに役に立つ。（例）どの「費目」があるかを見たいとき
<pre>select distinct カラム名 from テーブル名 ~</pre>
これだけselect文の後につなげるから注意。

- ORDER BY
<pre>select * from テーブル名
order by 出金額</pre>  
基本は昇順で並べられる。降順で並べたいときは、***order by 出金額 desc***  
また、複数の列を基準にして並べることもできる。最初指定した行で等しい行があったら次の指定した行で並び替えるということをする。  

- OFFSET-FETCH
<pre>select 費目、出金額 from 家計簿 order by 出金額
offset 0 rows
fetch next 3 rows only</pre>
offset-fetchは、order by句と同時に使われやすい。  
(MySQL, postgresqlの場合)offset-fetchは使わずに、Limit句を使う。


### 集合演算子  
複数のテーブルにselect文を送り、結果を足したりするときに使う。***ただし、それぞれのテーブルの列数とデータ型が違ってたら使えない。***
レコード同士を合わせて計算するため。
- union  
<pre>select 費目,入金額,出金額 from 家計簿 
union
select 費目,入金額,出金額 from 家計簿アーカイブ</pre>
unionの場合は、重複した行は取り除かれて算出される。union allの場合は、重複行もすべて算出して返す。  

- EXCEPT(MINUS) 
<pre>select 費目 from 家計簿
except
select 費目 from 家計簿アーカイブ</pre>
上のテーブルから下のテーブルと重複してるものを引く。これは順番が大事。  

- INTERSECT
<pre>select 費目 from 家計簿
INTERSECT 
select 費目 from 家計簿アーカイブ</pre>
それぞれのテーブルで重複してるものを取り出す。
