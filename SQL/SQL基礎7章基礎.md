# SQL第7章(副問い合わせ)  

副問い合わせを使いたい場合 => 「最大の出費に関する費目と金額をしりたい」場合など  
副問い合わせの例文<br>
<pre>select 費目, 出金額 from 家計簿  
where 出金額 =(select max(出金額) from 家計簿)
</pre>     
 このようにselect文が<strong>ネストされた状態</strong>になる。
 
 ### 副問い合わせが処理される仕組み  
 副問い合わせのselect文 => 外側のselect文 の順番で処理される。
 
 #### 副問い合わせの3つの代表的パターン<br>
 1.単一の値に化ける副問い合わせ<br>  
 基本的にどこでも記述することができる。代表的な例は、select文の選択列リストやfrom句、updateのset句、where句の条件式などがある。  
 
 2.列挙された複数値に化ける副問い合わせ<br>  
 複数の値を列挙するときに代わりに使用することができる。  
 代表的な例は、IN,ANY,ALl 演算子などである。<br>
 <pre>
 select * from 家計簿集計  
 where 費目 in (select distinct 費目 from 家計簿)
 </pre>  
 <strong>注意すべきこと</strong>  
 NOT IN演算子でNULLが含まれている場合、比較結果はすべてNULLとなってしまいだめ。<br>
 NULLを含んでいた場合の処理<br>
 <pre>  
 select * from 家計簿アーカイブ  
 where 費目 in (select 費目 from 家計簿 where 費目 is null)
 </pre>
  
 3.表形式の複数値に化ける副問い合わせ<br>
 select文のfrom句やinsert分のなどに記述することができる。  
 <pre>  
 select sum(sub.出金額) as 出金額合計  
 from (select 日付, 費目, 出金額 from 家計簿  
       union  
       select 日付, 費目, 出金額  from 家計簿アーカイブ  
       where 日付 >= '2018-01-01' and 日付 <= '2018-01-31') as sub  
 </pre>
 
