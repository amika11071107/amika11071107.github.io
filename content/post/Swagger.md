+++
author = "←この日付無視してね"
title = "Swagger  ⏰約20分"
date = "2024-12-28"
description = ""
+++


## 📚 [swagger editor](https://editor.swagger.io/)   
-  ↑まずはここをクリック

## 🌞 **Swaggerとは**  
🌷 オンラインのアプリを作るには、バックエンド（Webサーバ上で動くプログラム）を開発する必要があります  
🌷 バックエンドのコードを書く前に、何のデータをどんな形でサーバに送るのか、アカウントがなかったらどのようなエラーを返すか（404？401？)などを決めておく必要があります。これを Web API と言います  
🌷 Web API （厳密には RESTful API）を決めるために、Swaggerというツールが便利です。バックエンドのコードを書くときには Swagger で決めた内容を確認しながら書いていきます  

## 🖊 まずはパスの説明をします  
### 1. パスとは何か？
🌷 パスは、Web サーバー上で特定のデータや機能にアクセスするための「住所」のようなものです。  
たとえば：  

- 住所（パス）: localhost:8080/accounts  
- 意味: 「アカウントの情報がある場所」  
- これをブラウザやアプリが指定して、サーバーに「この住所にある情報をください」と頼む形です。  

### 2. 操作（HTTPメソッド）と組み合わせる  
🌷 パスは操作の種類と一緒に使います。操作の種類は「HTTPメソッド」と呼ばれる以下のものです：  

- GET: データを取得する（見るだけ）  
- POST: 新しいデータを追加する（作る）  
- PUT: データを更新する（書き換える）  
- DELETE: データを削除する（消す）  
  
### 3. 実際の例で説明  
**🌷 アカウント一覧を見る**  
- パス：localhost:8080/accounts  
- メソッド：GET  
→ 「アカウントが全部見たい！」と頼むと、一覧が返ってきます  

**🌷 新しいアカウントを作る**  
- パス：localhost:8080/accounts  
- メソッド：POST  
→ 「このアカウントIDとパスワードで新しいアカウントを作りたい！」と頼むと、サーバーが新しいアカウントを作り、保存してくれます  

**🌷 特定のアカウントの情報を見る**  
- パス：localhost:8080/accounts/abc  
- メソッド：GET  
→ 「abc というアカウントの情報が見たい！」と頼むと、そのアカウントの情報だけ返ってきます

**🌷 特定のアカウントのパスワードを変える**  
- パス：localhost:8080/accounts/abc/password  
- メソッド：PUT  
→ 「abc のアカウントのパスワードをこれに変更して！」と頼むと、サーバーがそのパスワードを更新します  

### 4. {account_id} の役割  
🌷 localhost:8080/accounts/{account_id} の {account_id} 部分は「特定のアカウントを指定するための場所」です  
🌷 実際には、この {account_id} の部分が具体的な値（例: abc や xyz）に置き換えられます  

例:  
- localhost:8080/accounts/abc → アカウント ID が abc の情報  
- localhost:8080/accounts/xyz → アカウント ID が xyz の情報  
これによって、「どのアカウントを操作するか」が分かる仕組みです  

### 5. まとめ：この仕組みのポイント  
🌷 パス: 「どの情報にアクセスするか」を指定する住所のようなもの  
🌷 HTTPメソッド: 「どんな操作をするか」を指定する命令  
🌷 パラメータ（例: {account_id}）: 「具体的にどのデータか」を特定する変数部分  
🌷 こうした仕組みを組み合わせて、サーバーはデータの管理や操作を効率的に行います  

## 🖊 次に実際に書いていきます     
🌷 以下は、Swagger editor (左) と Swagger UI (右) です    
🌷 Try our new Editorを押して、左のコードを全部消し、下の画像やコードをコピペしながら、UIと比べて理解していきましょう  
🌷 postとgetは同じ位置にあるかなどインデントに気をつけてコピペしていってください 
🌷 コピペをした際にエラーがとれなかったら再起動しましょう   

 ![images](/images/swagger6.png)

--------------------------------------------------------
  
🌷 右のSwagger UIと照らして見ていくと、、  

![images](/images/info.png)  

```yaml
openapi: 3.0.3

info:
  title: Lesson - OpenAPI 3.0  
  version: 1.0.0 

  description: |-
    bookチーム-分担1とか  
``` 

--------------------------------------------------------
![images](/images/tags.png)
```yaml
tags:
  - name: accounts
    description: 全アカウントの情報  # 詳しい説明を書きましょう
```
🌷タグの名前をaccountsとします📖

--------------------------------------------------------
![images](/images/required.png)
```yaml
components:
  parameters:  # {account_id}のようなものがパスパラメータ
    account_id:
      in: path
      name: account_id
      description: 取得対象のユーザーID
      schema:
        type: string
      required: true # 必須という意味
```

🌷{account_id}は後で何回も出てくるので、これパスパラメーターだよーとか、取得対象のユーザーidなんだよーとか、必須項目だよとかここに先に書いておきます。  
🌷後で「- $ref: "#/components/parameters/account_id"」と書いたら、{account_id}を使えます。

--------------------------------------------------------
![images](/images/swagger1.png)
```yaml 
paths:
  /accounts:
    get:
      tags:  # accountsタグの中のお話📖  
        - accounts
      summary: アカウントの一覧を返す  # 説明を書きましょう。
      description: アカウントの一覧をリストとして返す  # 詳しい説明を書きましょう
      responses:
        '200':  # 成功
          description: 成功
          content:
            application/json:  # JSON形式で返す
              schema:
                type: array  # 配列
                items:
                  type: string  # 文字列
                  example: id1
```

🌷/accountsというパスに対してGETをすると、成功だった場合（200)、アカウントの一覧をリストとしてJSON形式で返すことが書かれています。  
🌷schemaには返すデータの例がString（文字列）とarray（配列）を用いて表現されています。

--------------------------------------------------------
![images](/images/swagger2.png)
```yaml                
    post:
      tags:  # accountsタグの中のお話📖  
        - accounts
      summary: アカウントの新規作成  # 説明を書きましょう。
      description: account_idとpasswordを設定し新しいアカウントを作成する  # 詳しい説明を書きましょう
      requestBody:
        required: true  # repuestBodyは必須項目
        content:
          application/x-www-form-urlencoded:  # この形式でaccount_idとpasswordが登録される。
            schema:
              type: object  # object型データとはkeyとvalueで構成されるデータ。
              properties:
                account_id:
                  type: string
                  description: アカウント名
                password:
                  type: string
                  description: パスワード
              required:  # 必須項目
                - account_id
                - password
      responses:
        '200':  # 成功
          description: 成功
        '409':  # 競合
          description: すでにaccount_idが存在したとき

```

🌷/accountsというパスに対してPOSTをすると、成功だった場合（200)、x-www-form-urlencoded形式で、account_idとpasswordが keyとvalueで構成されるobject型データで登録されることが書かれています。    
🌷object型データというのは以下のような形  
  
```yaml
例 : 
{  
    "key"        :  "value",  
    "account_id" :  "nitta217lab",  
    "password"   :  "13nitta217"  
}  
```

--------------------------------------------------------
![images](/images/swagger3.png)
```yaml                      

  /accounts/{account_id}:
    get:
      tags:  # accountsタグの中のお話📖  
        - accounts
      summary: アカウント情報を返す(idとpassword)  # 説明を書きましょう。
      description: 指定されたアカウントの情報を返す  # 詳しい説明を書きましょう
      parameters:
        - $ref: "#/components/parameters/account_id"  # 初めに書いておいたパスパラメータのaccount_id
        - in: query  # passwordはqueryParameter
          name: password
          schema:
            type: string
          description: パスワード  # 詳しい説明を書きましょう
          required: true  # パスワードは必須項目
      responses:
        '200':
          description: 成功
          content:
            application/json:  # JSON形式で返す
              schema:
                type: object # object型データ
                properties:
                  account_id:
                    type: string
                    description: アカウントID
                    example: amika
                  password:
                    type: string
                    description: パスワード
                    example: abc123
        '401':
          description: パスワード間違い          
        '404':
          description: IDが存在しません
```

🌷queryParameterは、  
🌷 id,pass  
🌷/accounts{account_id}というパスに対してGETをすると、成功だった場合（200)、指定されたアカウントの情報（account_idとpassword）をJSON形式で返すことが書かれています。  
🌷schemaには返すデータの例がString（文字列）とarray（配列）を用いて表現されています。

--------------------------------------------------------
![images](/images/swagger5.png)
```yaml
  /accounts/{account_id}/password:
    put:
      tags:
        - accounts
      summary: パスワードを変更する  # 説明を書きましょう。
      description: 指定されたIDのアカウントを変更する  # 詳しい説明を書きましょう
      parameters:
        - $ref: "#/components/parameters/account_id"
      requestBody:
        required: true
        content:
          application/x-www-form-urlencoded: # この形式のデータを使ってパスワードを変更
            schema:
              type: object
              properties:
                password:
                  type: string
                  description: 変更前のパスワード
                new_password:
                  type: string
                  description: 新しいパスワード
              required:
                - password
                - new_password   
      responses:
        '401':
          description: パスワード間違い
```

🌷 FileタブのSave as YAML を押すと左のコードをyaml形式で保存できるため、このコードをチームで共有できます。  
🌷 他のチームのコードを開くときは、FileタブのImport fileを押し、保存したファイルを開くと閲覧や編集ができる。

以上でswaggerは終わり🎉  

2/5進んだよ🎊  何分かかかったかな？  休憩してね☕  

次はIntellij ideaでバックエンドのコードを書いていくよ！   








