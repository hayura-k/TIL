# metaタグ・OGP設定

metaタグは、SEO対策とOGPを表示するために用いられる。  

### SEOとは？

SEOは「検索エンジン最適化」の略で、Googleの検索順位を上位に表示させるための対策のこと。

### OGPとは？

OGPは、SNSなどでシェアする時などにアイキャッチ画像を表示させるために用いるためのHTMlの要素。

### 具体的な実装手順

1. railsでは**meta-tags**と言うgemを使用する。

2. application_helper.rbに記述する  

default_meta_tagsメソッドを作成し、その中にmetaタグで設定する内容を書く。  
```rb
def default_meta_tags
  {
     site: Settings.meta.site,
     reverse: true,
     title: Settings.meta.title,
     description: Settings.meta.description,
     keywords: Settings.meta.keywords,
     canonical: request.original_url,
     og: {
       title: :full_title,
       type: Settings.meta.og.type,
       url: request.original_url,
       image: image_url(Settings.meta.og.image_path),
       site_name: :site,
       description: :description,
       locale: 'ja_JP',
     },
     twitter: {
       card: 'summary_large_image',
     },
  }
end
```

site:サイト名  
title:タイトル名  
reverse: trueを設定することで、「タイトル|サイト名」の並びで出力する  
keywords: 検索した際にひっかかるワードを記述する  
canonical: URLの正規化のために設定するらしい  
[RailsでSEO上の問題からcanonicalタグを設定する方法 \- bagelee（ベーグリー）](https://bagelee.com/programming/ruby-on-rails/rails-seo-canonical/)  

og以下はSNSなどでシェアする際に表示されるものだ。  
type: website,articleなどから設定する  
image: アイキャッチ画像を表示  
locale:  リソースの言語を表示する  
他のsiteやtitleなどは同じである。  

3. headタグに記述  

metaタグを出力するためには、headタグの中にdisplay_meta_tags(default_meta_tags)とかいて表示する。  
ページ毎にタイトルなどを変えたいときは、set_meta_tagsメソッドを使う。

[【Rails】『meta\-tags』gemを使ってSEO対策をおこなう方法 \| vdeep](http://vdeep.net/rubyonrails-meta-tags-seo)  
[Railsアプリでmetaタグ＆OGP設定をする方法 \- 日常のあれこれ。](https://creat4869.hatenablog.com/entry/2019/08/15/170109)
