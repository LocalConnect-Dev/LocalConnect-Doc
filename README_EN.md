# Local Connect -  API Document
This shows usage of the Local Connect API.

## 1. Request and Response
- The client sends a request to the server with `application/www-form-urlencoded` type.
  The requests have parameters and headers according to the `HTTP/1.1` protocol.
- After the server proceeded the request, the server returns responses to the client with `application/json` type.
  For details, see the `RFC4627` standard.  
  [http://www.ietf.org/rfc/rfc4627](http://www.ietf.org/rfc/rfc4627)

## 2. Error
- When the server failed to proceed the request, it returns an error with the `Error` object.
  For details, see the specification of the object below.

### a. Error Codes
|Code|HTTP Status|Description|
|---|---|---|
|`NOT_AUTHORIZED`|401|When no session provided or the provided session is invalid.|
|`ENDPOINT_NOT_FOUND`|404|When the specified API endpoint is not found.|
|`TOKEN_NOT_FOUND`|404|When the provided token is invalid or expired.|
|`USER_NOT_FOUND`|404|When the user of the specified UUID is not found.|

## 3. Authentification and Authorization
- The user logs into the system with its token (may be as a QR code).
  At this time, the server provides a session to the client.
  Hereinafter, this is called **authentification** (authentificating).
- After the user logged in, when the client uses the API, the client creates a request with `X-LocalConnect-Session` header includes the session.
  At this time, the client gets restrictive permission (not permanently).
  Hereinafter, this is called **authorization** (authorizing).

## 4. Types
This shows the definition and the range of each types of values.

|Type|Range|Description|
|---|---|---|
|uint|0 - 4,294,967,295|A **unsigned** 32-bit numeric value.|
|ulong|0 - 18,446,744,073,709,551,615|A **unsigned** 64-bit numeric value.|
|string|N/A|A string value that contains chars of Unicode.|

## 5. Objects
The object shows a thing with member values.
**Objects can be Types in JSON.**

### a. `Error` Object
This shows an error.

|Member|Type|Description|
|---|---|---|
|error|string|An error code.|

### b. `Session` Object
This shows a session.

|Member|Type|Description|
|---|---|---|
|id|string|An UUID of the user.|
|user|User|A user that has the session.|
|created_at|ulong|A UNIX timestamp when the session created.|

### c. `User` Object
This shows a user.

|Member|Type|Description|
|---|---|---|
|id|string|A unique identifier (hereinafter, this is called **an UUID**) of the user.|
|name|string|A name of the user.|
|type|UserType|The type of the user.|
|group|Group|The group that the user belongs to.|
|created_at|ulong|A UNIX timestamp when the user created.|

## 6. Endpoints

### a. POST /sessions/create
This creates a session from the token.
This **doesn't need to authorize** .

#### i. Request Parameters
|Parameter|Type|Description|
|---|---|---|
|token|string|A token to create the session.|

#### ii. Response
This returns the created `Session` object with `200 OK` status.

### b. DELETE /sessions/destroy
This destroys the session.

#### i. Request Parameters
This needs **no** parameters.

#### ii. Response
This returns only `204 No Content` status.

### c. GET /users/me
This provides who is the authorized user with the current session.

#### i. Request Parameters
This needs **no** parameters.

#### ii. Response
This returns an `User` object of the current user.

## 7. Notes