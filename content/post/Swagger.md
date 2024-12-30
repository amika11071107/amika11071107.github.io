+++
author = "←この日付無視してね"
title = "Swagger"
date = "2024-12-28"
description = "説明かな？どこにも反映されてなさそうだ"
+++

コードを書く前に、何のデータをどんな形で送るのか、アカウントがなかったらどのようなエラーを出すか（404？401？)を決めてここに記し、コードを書くときにはSwaggerを見ながら書きます。

📚 [参考サイト](https://techblog.asahi-net.co.jp/entry/2019/03/04/102734)

[swagger editor](https://editor.swagger.io/) 
 
以下は、Swagger editor (左) と Swagger UI (右) です  
これを書いていきます（DELETE以外）
Try our new Editorを押して、左のコードを全部消し、下の画像や次の説明をコピペしながら、UIと比べて理解していきましょう

> ![images](/images/swagger6.png)

--------------------------------------------------------
  
右のSwagger UIと照らして見ていくと、、  

![images](/images/info.png)  

```yaml
openapi: 3.0.3

info:
  title: Citrus - OpenAPI 3.0  
  version: 1.0.0 

  description: |-
    bookチーム-分担1とか  
``` 
🌷 FileタブのSave as YAML を押すと左のコードをyaml形式で保存できるため、このコードをチームで共有できます。  
🌷 他のチームのコードを開くときは、FileタブのImport fileを押し、保存したファイルを開くと閲覧や編集ができる。

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
🌷object型データ  
  
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

🌷queryParameterは、リソースを絞り込むま条件や追加情報をURLで指定するための方法  
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


以上でswaggerは終わり🎉  

次はIntellij ideaでバックエンドのコードを書いていくよ！   






