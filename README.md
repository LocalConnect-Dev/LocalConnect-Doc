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

## 4. Permissions
- If the user wants to read/write (object of) users or of **another groups** , it **also need read/write_groups permission** .
- If the user wants to read/write (object of) users or groups of **another regions** , it **also need read/write_regions permission** .

|Permission|Description|
|---|---|
|read_permissions|Whether the user can read permissions.|
|read_types|Whether the user can read user types.|
|read_regions|Whether the user can read regions.|
|read_groups|Whether the user can read groups.|
|read_users|Whether the user can read users.|
|read_documents|Whether the user can read documents.|
|read_boards|Whether the user can read boards.|
|read_events|Whether the user can read events.|
|read_posts|Whether the user can read posts.|
|read_profiles|Whether the user can read profiles.|
|write_types|Whether the user can create or edit user types.|
|write_regions|Whether the user can create or edit regions.|
|write_groups|Whether the user can create or edit groups.|
|write_documents|Whether the user can create or edit documents.|
|write_users|Whether the user can create or edit users.|
|write_boards|Whether the user can create or edit boards.|
|write_events|Whether the user can create or edit events.|
|write_posts|Whether the user can post.|
|write_profiles|Whether the user can edit profiles.|

## 5. Types
This shows the definition and the range of each types of values.

|Type|Range|Description|
|---|---|---|
|bool|0 - 1|A boolean value. Also can be showed as `true` or `false`.|
|uint|0 - 4,294,967,295|A **unsigned** 32-bit numeric value.|
|ulong|0 - 18,446,744,073,709,551,615|A **unsigned** 64-bit numeric value.|
|string|N/A|A string value that contains chars of Unicode.|

## 6. Objects
The object shows a thing with member values.
**Objects can be Types in JSON.**

### a. `Error` Object
This shows an error.

|Member|Type|Description|
|---|---|---|
|error|string|An error code.|

### b. `Permission` object
This shows a permission.
it is a **read-only** object.

|Member|Type|Description|
|---|---|---|
|id|string|An UUID of the permission.|
|name|string|A name of the permission.|

### c. `UserType` Object
This shows a type of users.

|Member|Type|Description|
|---|---|---|
|id|string|An UUID of the type.|
|name|string|A name of the type. (Max. 255 chars)|
|permissions|Permission[]|An array of permissions that the user has.|
|created_at|ulong|A UNIX timestamp when the type created.|

### d. `Region` Object
This shows a region that has groups.

|Member|Type|Description|
|---|---|---|
|id|An UUID of the region.|
|name|A name of the region. (Max. 255 chars)|
|created_at|ulong|A UNIX timestamp when the region created.|

### e. `Group` Object
This shows a group in a region that has users.

|Member|Type|Description|
|---|---|---|
|id|An UUID of the group.|
|region|Region|A region that the group is in.|
|name|A name of the group. (Max. 255 chars)|
|created_at|ulong|A UNIX timestamp when the group created.|

### f. `User` Object
This shows a user.

|Member|Type|Description|
|---|---|---|
|id|string|A unique identifier (hereinafter, this is called **an UUID**) of the user.|
|name|string|A name of the user. (Max. 255 chars)|
|type|UserType|The type of the user.|
|group|Group|The group that the user belongs to.|
|created_at|ulong|A UNIX timestamp when the user created.|

### g. `CreatedUser` Object
This shows a created user.
**This inherits `User` object** .

|Member|Type|Description|
|---|---|---|
|token|string|A token of the created user.|

### h. `Session` Object
This shows a session.

|Member|Type|Description|
|---|---|---|
|id|string|An UUID of the user.|
|user|User|A user that has the session.|
|created_at|ulong|A UNIX timestamp when the session created.|

### i. `CreatedSession` Object
This shows a created session.
**This inherits `Session` object** .

|Member|Type|Description|
|---|---|---|
|secret|string|A secret of the created session.|


### j. `Document` Object
This shows a universal documentation.

|Member|Type|Description|
|---|---|---|
|id|string|An UUID of the document.|
|author|User|A user who created the document.|
|title|string|Title of the document. (Max. 512 chars)|
|content|string|Content of the document.|
|created_at|ulong|A UNIX timestamp when the document created.|

### k. `Board` Object
This shows a bulletin board.

|Member|Type|Description|
|---|---|---|
|id|string|An UUID for the board.|
|group|Group|A group that the board provided in.|
|documents|Document[]|Array of documents in the board.|
|created_at|ulong|A UNIX timestamp when the board created.|


### l. `BoardRead` Object
This shows a user-read mark of a board.

|Member|Type|Description|
|---|---|---|
|id|string|An UUID of the mark.|
|user|User|A user who read the board.|
|board|Board|A board that the user read.|
|created_at|ulong|A UNIX timestamp when the user read the board.|

### m. `Event` Object
This shows a event.

|Member|Type|Description|
|---|---|---|
|id|string|An UUID of the event.|
|author|User|A user who created the event.|
|document|Document|A document that is described the event.|
|date|ulong|A UNIX timestamp when the event will start.|
|created_at|ulong|A UNIX timestamp when the event created.|

### n. `EventAttendance` Object
This shows a attendance of a event and a user.

|Member|Type|Description|
|---|---|---|
|id|string|An UUID of the attendance.|
|user|User|A user of the attendee.|
|event|Event|A event that the attendee joins.|
|created_at|ulong|A UNIX timestamp when the attendee decided to join.|

### o. `Post` Object
This shows a user post.

|Member|Type|Description|
|---|---|---|
|id|string|An UUID of the post.|
|author|User|A user who posted.|
|document|Document|A document of the content of the post.|
|created_at|ulong|A UNIX timestamp when the post created.|

### p. `PostLike` Object
This shows a "Like!" reaction of a post.

|Member|Type|Description|
|---|---|---|
|id|string|An UUID of the reaction.|
|user|User|A user who liked.|
|post|Post|A board that the user liked.|
|created_at|ulong|A UNIX timestamp when the user liked the post.|

### q. `Profile` Object
This shows a profile of a user.

|Member|Type|Description|
|---|---|---|
|id|string|An UUID of the user.|
|user|User|A user that has the profile.|
|hobbies|string|Hobbies of the user. (Max. 512 chars)|
|favorites|string|Favorite things of the user. (Max. 512 chars)|
|mottoes|string|Mottoes of the user. (Max. 512 chars)|
|updated_at|ulong|A UNIX timestamp when the profile last updated.|

## 7. Endpoints

### a. GET /permissions/show
This provides a permission.

#### i. Request Parameters
|Parameter|Type|Description|
|---|---|---|
|id|string|The UUID of the permission to show.|

#### ii. Response
This returns a `Permission` object.

### b. GET /permissions/list
This provides a list of permissions.

#### i. Request Parameters
This needs **no** parameters.

#### ii. Response
This returns an array of `Permissions` objects.

### c. GET /types/show
This provides a user type.
This requires **read_types** permission.

#### i. Request Parameters
|Parameter|Type|Description|
|---|---|---|
|id|string|The UUID of the type to show.|

#### ii. Response
This returns a `UserType` object.

### d. GET /types/list
This provides a list of user types.
This requires **read_types** permission.

#### i. Request Parameters
This needs **no** parameters.

#### ii. Response
This returns an array of `UserType` objects.

### e. POST /types/create
This creates or edits a user type.
This requires **write_types** permission.

#### i. Request Parameters
|Parameter|Type|Description|
|---|---|---|
|id|string|Optional. If this is provided, the user can edit the type.|
|permissions|string (comma-separated array)|Permissions that the user of the type has.|

#### ii. Response
This returns the `UserType` object that the user created or edited.

### f. GET /regions/show
This provides a region.
This requires **read_regions** permission.

#### i. Request Parameters
|Parameter|Type|Description|
|---|---|---|
|id|string|The UUID of the region to show.|

#### ii. Response
This returns a `Region` object.

### g. GET /regions/list
This provides a list of regions.
This requires **read_regions** permission.

#### i. Request Parameters
This needs **no** parameters.

#### ii. Response
This returns an array of `Region` objects.

### h. POST /regions/create
This creates or edits a region.
This requires **write_regions** permission.

#### i. Request Parameters
|Parameter|Type|Description|
|---|---|---|
|id|string|Optional. If this is provided, the user can edit the region.|
|name|string|A name of the region.|

### i. GET /groups/show
This provides a group.
This requires **read_groups** permission.

#### i. Request Parameters
|Parameter|Type|Description|
|---|---|---|
|id|string|The UUID of the group to show.|

#### ii. Response
This returns a `Group` object.

### j. GET /groups/list
This provides a list of groups in a region.
This requires **read_groups** permission.

#### i. Request Parameters
|Parameter|Type|Description|
|---|---|---|
|region|string|Optional. The UUID of the region to list groups. (Default: the region of current user)|

#### ii. Response
This returns an array of `Group` objects.

### k. POST /groups/create
This creates or edits a group.
This requires **write_groups** permission.

#### i. Request Parameters
|Parameter|Type|Description|
|---|---|---|
|id|string|Optional. If this is provided, the user can edit the group.|
|name|string|A name of the group.|

#### ii. Response
This returns the `Group` object that the user created or edited.

### l. GET /users/me
This provides who is the authorized user with the current session.

#### i. Request Parameters
This needs **no** parameters.

#### ii. Response
This returns an `User` object of the current user.

### m. GET /users/show
This provides a user.
This requires **read_users** permission.

#### i. Request Parameters
|Parameter|Type|Description|
|---|---|---|
|id|string|The UUID of the user to show.|

#### ii. Response
This returns an `User` object.

### n. GET /users/list
This provides a list of users in a group.
This requires **read_users** permission.

#### i. Request Parameters
|Parameter|Type|Description|
|---|---|---|
|group|string|Optional. The UUID of the group to list users. (Default: the group of current user)|

#### ii. Response
This returns an array of `User` objects.

### o. POST /users/create
This creates or edits a user.
This requires **write_users** permission.

#### i. Request Parameters
|Parameter|Type|Description|
|---|---|---|
|id|string|Optional. If this is provided, it can edit the user.|
|name|string|The name of the user.|
|type|string|The UUID of the user type.|
|group|string|The UUID of the group that the user belongs to.|

#### ii. Response
This returns the `CreatedUser` object that the user created or edited.

### p. GET /sessions/current
This provides the current session.

#### i. Request Parameters
This needs **no** parameters.

#### ii. Response
This returns the `Session` object of the current session with `200 OK` status.

### q. POST /sessions/create
This creates a session from the token.
This **doesn't need to authorize** .

#### i. Request Parameters
|Parameter|Type|Description|
|---|---|---|
|token|string|A token to create the session.|

#### ii. Response
This returns the created `CreatedSession` object with `200 OK` status.

### r. DELETE /sessions/destroy
This destroys the session.

#### i. Request Parameters
This needs **no** parameters.

#### ii. Response
This returns only `204 No Content` status.

### s. GET /documents/show
This provides a document.
This requires **read_documents** permission.

#### i. Request Parameters
|Parameter|Type|Description|
|---|---|---|
|id|string|The UUID of the document to show.|

#### ii. Response
This returns an `Document` object.

### t. GET /documents/list
This provides a list of documents of a user.
This requires **read_documents** permission.

#### i. Request Parameters
|Parameter|Type|Description|
|---|---|---|
|user|string|Optional. The UUID of the user to list documents. (Default: current user)|

#### ii. Response
This returns an array of `Document` objects.

### u. POST /documents/create
This creates or edits a document.
This requires **write_documents** permission.

#### i. Request Parameters
|Parameter|Type|Description|
|---|---|---|
|id|string|Optional. If this is provided, the user can edit the document.|
|title|string|A title of the document.|
|content|string|A content of the document.|

#### ii. Response
This returns the `Document` object that the user created or edited.

### v. GET /boards/show
This provides a board.
This requires **read_boards** permission.

#### i. Request Parameters
|Parameter|Type|Description|
|---|---|---|
|id|string|The UUID of the board to show.|

#### ii. Response
This returns an `Board` object.

### w. GET /boards/reads
This provides who read the board.
This requires **read_boards** permission.

#### i. Request Parameters
|Parameter|Type|Description|
|---|---|---|
|id|string|The UUID of the board to show read marks.|

#### ii. Response
This returns an array of `BoardRead` objects.

### x. GET /boards/list
This provides a list of boards of a group.
This requires **read_boards** permission.

#### i. Request Parameters
|Parameter|Type|Description|
|---|---|---|
|group|string|Optional. The UUID of the group to list boards. (Default: current group)|

#### ii. Response
This returns an array of `Board` objects.

### y. POST /boards/read
This mark a board read.
This requires **read_boards** permission.

#### i. Request Parameters
|Parameter|Type|Description|
|---|---|---|
|board|string|The UUID of the board to mark.|

#### ii. Response
This returns a `BoardRead` object that the user marked.

### z. POST /boards/create
This creates or edits a board.
This requires **write_boards** permission.

#### i. Request Parameters
|Parameter|Type|Description|
|---|---|---|
|id|string|Optional. If this is provided, the user can edit the board.|
|documents|string (comma-separated array)|UUIDs of documents that the board has.|

#### ii. Response
This returns the `Board` object that the user created or edited.

### aa. GET /events/show
This provides a event.
This requires **read_events** permission.

#### i. Request Parameters
|Parameter|Type|Description|
|---|---|---|
|id|string|The UUID of the event to show.|

#### ii. Response
This returns an `Event` object.

### ab. GET /events/list_user
This provides a list of events that a user hold.
This requires **read_events** permission.

#### i. Request Parameters
|Parameter|Type|Description|
|---|---|---|
|user|string|Optional. The UUID of the user to list events. (Default: current user)|

#### ii. Response
This returns an array of `Event` objects.

### ac. GET /events/list_group
This provides a list of events of a group.
This requires **read_events** permission.

#### i. Request Parameters
|Parameter|Type|Description|
|---|---|---|
|group|string|Optional. The UUID of the group to list events. (Default: the group of current user)|

#### ii. Response
This returns an array of `Event` objects.

### ad. POST /events/join
This creates attendance of a event and a user.
This requires **read_events** permission.

#### i. Request Parameters
|Parameter|Type|Description|
|---|---|---|
|event|string|The UUID of the event to mark.|

#### ii. Response
This returns the `EventAttendance` object.

### ae. POST /events/create
This creates or edits a event.
This requires **write_events** permission.

#### i. Request Parameters
|Parameter|Type|Description|
|---|---|---|
|id|string|Optional. If this is provided, the user can edit the event.|
|document|string|The UUID of the document that is explained the event.|
|date|ulong|A UNIX timestamp when the event will start.|

#### ii. Response
This returns the `Event` object that the user created or edited.

### af. GET /posts/show
This provides a post.
This requires **read_posts** permission.

#### i. Request Parameters
|Parameter|Type|Description|
|---|---|---|
|id|string|The UUID of the post to show.|

#### ii. Response
This returns an `Post` object.

### ag. GET /posts/likes
This provides who liked the post.
This requires **read_posts** permission.

#### i. Request Parameters
|Parameter|Type|Description|
|---|---|---|
|id|string|The UUID of the post to show the reactions.|

#### ii. Response
This returns an array of `PostLike` objects.

### ah. GET /posts/list_user
This provides a list of posts of a user.
This requires **read_posts** permission.

#### i. Request Parameters
|Parameter|Type|Description|
|---|---|---|
|user|string|Optional. The UUID of the user to list posts. (Default: current user)|

#### ii. Response
This returns an array of `Post` objects.

### ai. GET /posts/list_group
This provides a list of posts of a user.
This requires **read_posts** permission.

#### i. Request Parameters
|Parameter|Type|Description|
|---|---|---|
|group|string|Optional. The UUID of the group to list posts. (Default: current group)|

#### ii. Response
This returns an array of `Post` objects.

### aj. POST /posts/like
This likes a post.
This requires **read_posts** permission.

#### i. Request Parameters
|Parameter|Type|Description|
|---|---|---|
|post|string|The UUID of the post to like.|

#### ii. Response
This returns a `PostLike` object that the user reacted.

### ak. POST /posts/create
This creates or edits a post.
This requires **write_posts** permission.

#### i. Request Parameters
|Parameter|Type|Description|
|---|---|---|
|id|string|Optional. If this is provided, the user can edit the post.|
|document|string|The UUID of the document of the post.|

#### ii. Response
This returns the `Post` object that the user created or edited.

### al. GET /profiles/mine
This provides a profile of the user who has the current session.

#### i. Request Parameters
This needs **no** parameters.

#### ii. Response
This returns an `Profile` object of the profile of the current user.

### am. GET /profiles/show
This provides a profile.
This requires **read_profiles** permission.

#### i. Request Parameters
|Parameter|Type|Description|
|---|---|---|
|id|string|The UUID of the profile to show.|

#### ii. Response
This returns an `Profile` object.

### an. GET /profiles/lookup
This provides a profile of a user.
This requires **read_profiles** permission.

#### i. Request Parameters
|Parameter|Type|Description|
|---|---|---|
|user|string|The UUID of the user to show the profile.|

#### ii. Response
This returns an `Profile` object.

### ao. POST /profiles/create
This creates or edits a profile of the current user.
This requires **write_posts** permission.

#### i. Request Parameters
|Parameter|Type|Description|
|---|---|---|
|hobbies|string|Hobbies of the user. (Max. 512 chars)|
|favorites|string|Favorite things of the user. (Max. 512 chars)|
|mottoes|string|Mottoes of the user. (Max. 512 chars)|

#### ii. Response
This returns the `Profile` object that the user created or edited.

## 8. Notes