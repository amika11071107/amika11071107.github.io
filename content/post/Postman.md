---
author: ←この日付無視してね
title: Postman
date: 2019-03-08
description: あ
math: true
---

端末に情報を入力したときに、何が返ってきているのかを可視化するためのツール

[PostMan](https://www.postman.com/downloads/)

どうにかして新規登録をすると、下の画面が出てきます  
![images](/images/postman1.png)
*↑{{< color  deeppink "〇を押します" >}}*  
.  
.  
![images](/images/postman2.png)
*↑{{< color  deeppink "□を押すと、" >}}*  
.  
.  
↓{{< color  purple "GET POST PUT DELが出てきます" >}}  
![images](/images/postman3.png)
*↑{{< color  deeppink "POSTを押す" >}}*  
.  
.   
## Post 
↓{{< color  deeppink "Bodyを選択" >}}    
↓{{< color  deeppink "x-www-form-urlencodedを選択" >}}    
↓{{< color  deeppink "アカウントとパスワード" >}} の {{< color  purple "KeyとValueを入力するための欄が出てきます" >}}  
↓{{< color  purple "Key" >}} に {{< color  green "account_id ,  password" >}}と{{< color  deeppink "入力" >}}  
↓{{< color  purple "Value" >}} に 自分のつけたいアカウント名とパスワードを{{< color  deeppink "入力" >}}  
（これがフォームデータとして {{< color  deeppink "x-www-form-urlencoded" >}} 形式で送信される  
**Intellij起動**して、  
↓{{< color  deeppink "Sendを押すと" >}}
↓{{< color  purple "tokenが出てきます" >}}   
・tokenは登録したアカウント情報をGETするときや、アカウントをDELETEするときに利用する。  
・2回目にpostしたときは、「（入力したアカウントid）は既に存在します」と表示され、tokenは表示されないから、一回目にtokenはコピペしとく   
**{{< color  yellowgreen "localhost:8080" >}}**   
・**localhost**はコンピュータ自信を指す特別なホスト名  
・IPアドレスに置き換えると127.0.0.1と同じ  
・**8080**はポート番号  
![images](/images/postman6.png)
**AccountRest.java**  
POSTは、@Consumesがいる
![images](/images/post1.png)

## GET  
{{< color  yellowgreen "{ここに" >}}{{< color  green "accountId" >}}{{< color  yellowgreen "入れる}。" >}}  
下は{{< color  deeppink "token" >}}だけ
![images](/images/postman7.png)  
*今はまだ何も情報を入れていないから、返ってくる{{< color  purple "introduction は null" >}} になっている*  

**AccountRest.java**  
![images](/images/get1.png)
AccountManager.java  

![images](/images/amga.png)  
*②作ったHashMap（accounts）に、アカウントIDを与えてaccounts.get(accountId)と書くことでそのアカウント情報を取ってくれる*  

![images](/images/amhm.png)  
*①アカウントIDとそれに対応するアカウント情報（Account）がまとまったHashMapを作成し、accountsに代入*  

**Account.java**  
![images](/images/aj.png)

以上でPostmanは終わり🎉

次はAndroidStudioでフロントエンドを書いていくよ！
