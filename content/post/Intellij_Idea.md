+++
author = "←この日付無視してね"
title = "Intellij IDEA  ⏰約20分 "
date = "2024-12-27"
description = "ここはどこに表示されているのだろう"

+++

## 🌞 **バックエンドを書くよ！**  
🌷 それでは、バックエンドのコードを書いていきましょう  
🌷 バックエンドのコードは、IntelliJ IDEA の Ultimate （学生および教職員向けの個人ライセンス）を使って書いていきます    

🌷 以下は19thのシトラスプロジェクトです🍊  
🌷 このように、プロジェクトでは entities, repositories, resources という3つのPackageを作り、その中に 	javaファイルを作っていきます  
🌷 他のプロジェクトが開いた状態になっている人は、左上の4本線をクリックし、fileからclose projectを押します。
![images](/images/in.png)  

-  
-  
-  
## 🖊 作っていきます  
🌷 以下の画面になると、まずはNewProjectを押します
![images](/images/in0.png)

-  
-  
-  
🌷 左の Spring Bootを選択し、JDK は 21 以上にします  
🌷 Name は LessonServer と設定します  
🌷 Next を押し、
![images](/images/in1.png)

-  
-  
-  
🌷 Jersey を選択し、Create を押します
![images](/images/in2.png)

-  
-  
-  

🌷 LessonServer を右クリックし、その下に entities と、resources という名前の Package を作ります  

![images](/images/in4.png)

-  
-  
-  
🌷 entitiesの中には、Account.java を作ります（画像では repositories パッケージが作られてしまっていますが無視してください）
![images](/images/in5.png)

-  
-  
-  
🌷 Account と入力します
![images](/images/in6.png)

-  
-  
-  
🌷 resorces の中には、AccountsRest.javaを作ります    
🌷 entities や resources と同じ位置に JerseyConfig を作ります  
![images](/images/in7.png)

-  
-  
-  
🌷 JerseyConfig には以下をコピペしてください  
```java
package com.example.LessonServer;

import org.glassfish.jersey.server.ResourceConfig;
import org.springframework.stereotype.Component;

@Component
public class JerseyConfig extends ResourceConfig {

    public JerseyConfig() {
        packages("com.example.LessonServer.resources");
    }
}  
```

-  
-  
-  
🌷 Account.javaには以下をコピペしてください  
🌷このクラスは指定されたアカウントの情報（account_id と password）を返すために使われます  

```java
package com.example.LessonServer.entities;

package com.example.LessonServer.entities;

public class Account {
    private String account_id;
    private String password;

    public Account(String aid, String pass) {
        account_id = aid;
        password = pass;
    }

    public String getAccount_id() {
        return account_id;
    }

    public void setAccount_id(String account_id) {
        this.account_id = account_id;
    }

    public void setPassword(String p) {
        password = p;
    }

    public String getPassword() {
        return password;
    }

}

```

-  
-  
-  
🌷 AccountRest.javaには以下のコードをコピペしてください  
🌷 このクラスはSwaggerで定義したAPIに対応しています  
-  例 :   
@Path("/accounts") はパスを表す   
@GET や @POST はHTTPメソッドを表しています  
  
🌷 @PUT や @POST でパラメータを受け取るときは  
-  @Consumes(MediaType.APPLICATION_FORM_URLENCODED) をつける  
  
🌷 レスポンスを返すときは  
-  @Produces(MediaType.APPLICATION_JSON) を付ける    
```java
package com.example.LessonServer.resources;

import com.example.LessonServer.entities.Account;
import jakarta.ws.rs.*;
import jakarta.ws.rs.core.*;
import org.springframework.stereotype.Component;
import java.util.HashMap;
import java.util.Set;

@Path("/accounts")
@Component
public class AccountsRest {
    // key に accountId, value に Account の HashMap を作る
    private final HashMap<String, Account> accounts = new HashMap<String, Account>();

    // アカウントの一覧をリストとして返す(GET)
    @GET
    @Produces(MediaType.APPLICATION_JSON)
    public Set<String> getAccounts() {
        return accounts.keySet();  // この書き方で、アカウントの一覧をリストとして返せる
    }

    // account_idとpasswordを設定し新しいアカウントを作成する(POST)
    @POST
    @Consumes(MediaType.APPLICATION_FORM_URLENCODED)  // bodyに入力する値がある時
    public void signup(@FormParam("account_id") String accountId, @FormParam("password") String password) {
        // パスワードが入力されていない(null)の場合
        if (password == null) {
            var response = Response.status(Response.Status.BAD_REQUEST) // 404
                    .entity("passwordを入力してください"); // postmanにこう表示される
            throw new WebApplicationException(response.build());
        } else {
            // アカウントが既に存在した場合
            if (accounts.containsKey(accountId)) {
                var response = Response.status(Response.Status.CONFLICT)  // 409
                        .entity("id '" + accountId + "' は既に存在します");  // postmanにこう表示される
                throw new WebApplicationException(response.build());
            }
            // アカウント作成の処理 200
            Account account = new Account(accountId, password);
            accounts.put(accountId, account);
        }
    }

    // 指定されたアカウントの情報を返す(GET)
    @Path("/{account_id}")
    @GET
    @Produces(MediaType.APPLICATION_JSON)
    public Account getAccountInfo(@PathParam("account_id") String accountId, @QueryParam("password") String password) {
        // account_idが存在しない場合 404
        if (!accounts.containsKey(accountId)) {
            var response = Response.status(Response.Status.NOT_FOUND).entity("IDが存在しません");
            throw new WebApplicationException(response.build());
        }
        Account account = accounts.get(accountId);
        // passwordが存在しない場合 401
        if (!account.getPassword().equals(password)) {
            var response = Response.status(Response.Status.UNAUTHORIZED).entity("パスワードが間違っています");
            throw new WebApplicationException(response.build());
        }
        // アカウント情報を返す 200
        return account;
    }

    // passwordを変更する(PUT)
    @Path("/{account_id}/password")
    @PUT
    @Consumes(MediaType.APPLICATION_FORM_URLENCODED)  // bodyに入力する値がある時
    @Produces(MediaType.APPLICATION_FORM_URLENCODED)
    public void changePW(@FormParam("new_password") String newPassword,
                                    @FormParam("old_password") String oldPassword,
                                    @PathParam("account_id") String accountId) {
        // パスワードが間違っている場合 401
        if (!accounts.get(accountId).getPassword().equals(oldPassword)) {
            var response = Response.status(Response.Status.UNAUTHORIZED)
                    .entity("パスワードが違います");
            throw new WebApplicationException(response.build());
        }
        // パスワードを置き換える 200
        accounts.get(accountId).setPassword(newPassword);
    }
}

```

以上でIntellij ideaは終わり🎉  

3/5終わったよ🎊  何分かかかったかな？  休憩してね☕

次はこのコードが動いているかPostmanで試していくよ！