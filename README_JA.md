# Local Connect -  API ドキュメント
Local Connect API の使用方法を示します。

## 1. リクエストとレスポンス
- クライアントはサーバへリクエストを `application/www-form-urlencoded` 形式で送信します。
  リクエストは `HTTP/1.1` プロトコルに基づいたパラメータ及びヘッダを持ちます。
- サーバがリクエストを処理した後、サーバはクライアントへレスポンスを `application/json` 形式で返します。
  JSON形式の詳細は、 `RFC4627` 規格をご覧ください。  
  [http://www.ietf.org/rfc/rfc4627](http://www.ietf.org/rfc/rfc4627)

## 2. エラー
- サーバがリクエストの処理に失敗したとき、サーバは `Error` オブジェクトを返します。
  詳細は、後述する 5. オブジェクト の `Error` オブジェクトをご覧ください。

### a. エラーコードの一覧
|コード|HTTP ステータス|説明|
|---|---|---|
|`NOT_AUTHORIZED`|401|ヘッダで指定されたセッションが正しくない、またはセッションが指定されていないとき。|
|`ENDPOINT_NOT_FOUND`|404|リクエストされた API エンドポイントが存在しないとき。|
|`TOKEN_NOT_FOUND`|404|指定されたトークンが正しくないまたは期限切れのとき。|
|`USER_NOT_FOUND`|404|指定されたUUIDのユーザが存在しないとき。|

## 3. 認証と承認
- ユーザはQRコードなどで示されたトークンを用いて、システムにログインします。
  このとき、サーバはクライアントにセッションを付与します。
  これを **認証** (Authentification) と呼びます。
- ユーザがログインした後、クライアントがAPIを使用するとき、クライアントはリクエストを `X-LocalConnect-Session` ヘッダにセッションを含めて作成します。
  このとき、クライアントは永続的でない限定的な権限を持ちます。
  これを **承認** (Authorization) と呼びます。

## 4. 型
それぞれの値の種類について、定義と取り得る値の範囲を示します。

|型|範囲|説明|
|---|---|---|
|uint|0 - 4,294,967,295|32ビットで表される **0以上の** 数値。|
|ulong|0 - 18,446,744,073,709,551,615|64ビットで表される **0以上の** 数値。|
|string|N/A|Unicode文字で表される文字列。|

## 5. オブジェクト
メンバ値を持って、ある物を表現します。
**JSON オブジェクト内では、オブジェクトも型となります。**

### a. `Error` Object
エラーを示します。

|メンバ|型|説明|
|---|---|---|
|error|string|エラーコード。|

### b. `Session` オブジェクト
セッションを示します。

|メンバ|型|説明|
|---|---|---|
|id|string|セッションのUUID。|
|user|User|セッションを持つユーザ。|
|created_at|ulong|セッションが作成されたときのUNIXタイムスタンプ。|

### c. `User` オブジェクト
ユーザを示します。

|メンバ|型|説明|
|---|---|---|
|id|string|ユーザ固有の識別子 (**UUID** といいます) 。|
|name|string|ユーザの名前。|
|type|UserType|ユーザの種類。|
|group|Group|ユーザが所属するグループ。|
|created_at|ulong|ユーザが作成されたときのUNIXタイムスタンプ。|

## 6. エンドポイント

### a. POST /sessions/create
トークンからセッションを作成します。
**承認は必要ありません** 。

#### i. リクエストパラメータ
|パラメータ|型|説明|
|---|---|---|
|token|string|セッションを作成するためのトークン。|

#### ii. レスポンス
作成された `Session` オブジェクト及び `200 OK` ステータスを返します。

### b. DELETE /sessions/destroy
セッションを破棄します。

#### i. リクエストパラメータ
パラメータは **不要** です。

#### ii. レスポンス
`204 No Content` ステータスのみを返します。

### a. GET /users/me
現在のセッションで承認されたユーザを返します。

#### i. リクエストパラメータ
パラメータは **不要** です。

#### ii. レスポンス
現在のユーザの `User`オブジェクトを返します。

## 7. 注釈