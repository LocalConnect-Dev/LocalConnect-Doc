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

### b. `UserType` Object
This shows a permission type of users.

|Member|Type|Description|
|---|---|---|
|id|string|An UUID of the type.|
|name|string|A name of the type. (Max. 255 chars)|
|read_boards|bool|Whether the user can read boards.|
|read_events|bool|Whether the user can read events.|
|read_posts|bool|Whether the user can read posts.|
|read_profiles|bool|Whether the user can read profiles.|
|write_types|bool|Whether the user can create or edit permission types.|
|write_regions|bool|Whether the user can create or edit regions.|
|write_groups|bool|Whether the user can create or edit groups.|
|write_users|bool|Whether the user can create or edit users.|
|write_boards|bool|Whether the user can create or edit boards.|
|write_events|bool|Whether the user can create or edit events.|
|write_posts|bool|Whether the user can post.|
|write_profiles|bool|Whether the user can edit profiles.|
|created_at|ulong|A UNIX timestamp when the type created.|

### c. `Region` Object
This shows a region that has groups.

|Member|Type|Description|
|---|---|---|
|id|An UUID of the region.|
|name|A name of the region. (Max. 255 chars)|
|created_at|ulong|A UNIX timestamp when the region created.|

### d. `Group` Object
This shows a group in a region that has users.

|Member|Type|Description|
|---|---|---|
|id|An UUID of the group.|
|region|Region|A region that the group is in.|
|name|A name of the group. (Max. 255 chars)|
|created_at|ulong|A UNIX timestamp when the group created.|

### e. `User` Object
This shows a user.

|Member|Type|Description|
|---|---|---|
|id|string|A unique identifier (hereinafter, this is called **an UUID**) of the user.|
|name|string|A name of the user. (Max. 255 chars)|
|type|UserType|The type of the user.|
|group|Group|The group that the user belongs to.|
|created_at|ulong|A UNIX timestamp when the user created.|

### f. `CreatedUser` Object
This shows a created user.
**This inherits `User` object** .

|Member|Type|Description|
|---|---|---|
|token|string|A token of the created user.|

### g. `Session` Object
This shows a session.

|Member|Type|Description|
|---|---|---|
|id|string|An UUID of the user.|
|user|User|A user that has the session.|
|created_at|ulong|A UNIX timestamp when the session created.|

### h. `CreatedSession` Object
This shows a created session.
**This inherits `Session` object** .

|Member|Type|Description|
|---|---|---|
|secret|string|A secret of the created session.|


### i. `Document` Object
This shows a universal documentation.

|Member|Type|Description|
|---|---|---|
|id|string|An UUID of the document.|
|author|User|A user who created the document.|
|title|string|Title of the document. (Max. 512 chars)|
|content|string|Content of the document.|
|created_at|ulong|A UNIX timestamp when the document created.|

### j. `Board` Object
This shows a bulletin board.

|Member|Type|Description|
|---|---|---|
|id|string|An UUID for the board.|
|group|Group|A group that the board provided in.|
|documents|Document[]|Array of documents in the board.|
|created_at|ulong|A UNIX timestamp when the board created.|


### k. `BoardRead` Object
This shows a user-read mark of a board.

|Member|Type|Description|
|---|---|---|
|id|string|An UUID of the mark.|
|user|User|A user who read the board.|
|board|Board|A board that the user read.|
|created_at|ulong|A UNIX timestamp when the user read the board.|

### l. `Event` Object
This shows a event.

|Member|Type|Description|
|---|---|---|
|id|string|An UUID of the event.|
|author|User|A user who created the event.|
|document|Document|A document that is described the event.|
|date|ulong|A UNIX timestamp when the event will start.|
|created_at|ulong|A UNIX timestamp when the event created.|

### m. `Post` Object
This shows a user post.

|Member|Type|Description|
|---|---|---|
|id|string|An UUID of the post.|
|author|User|A user who posted.|
|document|Document|A document of the content of the post.|
|created_at|ulong|A UNIX timestamp when the post created.|

### n. `PostLike` Object
This shows a "Like!" reaction of a post.

|Member|Type|Description|
|---|---|---|
|id|string|An UUID of the reaction.|
|user|User|A user who liked.|
|post|Post|A board that the user liked.|
|created_at|ulong|A UNIX timestamp when the user liked the post.|

### o. `Profile` Object
This shows a profile of a user.

|Member|Type|Description|
|---|---|---|
|id|string|An UUID of the user.|
|user|User|A user that has the profile.|
|hobbies|string|Hobbies of the user. (Max. 512 chars)|
|favorites|string|Favorite things of the user. (Max. 512 chars)|
|mottoes|string|Mottoes of the user. (Max. 512 chars)|
|updated_at|ulong|A UNIX timestamp when the profile last updated.|

## 6. Endpoints

### a. GET /sessions/current
This provides the current session.

#### i. Request Parameters
This needs **no** parameters.

#### ii. Response
This returns the `Session` object of the current session with `200 OK` status.

### b. POST /sessions/create
This creates a session from the token.
This **doesn't need to authorize** .

#### i. Request Parameters
|Parameter|Type|Description|
|---|---|---|
|token|string|A token to create the session.|

#### ii. Response
This returns the created `CreatedSession` object with `200 OK` status.

### c. DELETE /sessions/destroy
This destroys the session.

#### i. Request Parameters
This needs **no** parameters.

#### ii. Response
This returns only `204 No Content` status.

### d. GET /users/me
This provides who is the authorized user with the current session.

#### i. Request Parameters
This needs **no** parameters.

#### ii. Response
This returns an `User` object of the current user.

### e. POST /users/create
This creates a user.

#### i. Request Parameters
`region` or `group` is required.
**`group` overrides `region`** .

|Parameter|Type|Description|
|---|---|---|
|region|string|The UUID of the region that the user belongs to.|
|group|string|The UUID of the group that the user belongs to.|
|name|string|The name of the user.|

#### ii. Response
This returns `CreatedUser` object.

### f. GET /profiles/mine
This provides a profile of the user who has the current session.

#### i. Request Parameters
This needs **no** parameters.

#### ii. Response
This returns an `Profile` object of the profile of the current user.

## 7. Notes