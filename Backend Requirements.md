# Backend Requirements
Backend is only an API service without any rendering, and should just focus on producing JSON for client consumption.

# Technology
- Database
    - MongoDB

- Cache/Meta
    - Redis

# Services
- Login
    - Facebook

- Email
    - Mandrill
    - Sendinblue
    - Mailgun

- Embedded Media
    - Embedly

- Comment
    - Disqus

# SLA
- Maximum response time
    - TBA

- Minimum request per seconds load Capacity
    - TBA

- Minimum concurrent users load capacity
    - TBA

------------------------------------------------------

# Metas

```
Logo (http://your_url/example.jpeg)

Background (http://your_url/example.jpeg)

Theme (theme_name)

ColorScheme (#aaabbb,#ccceee,#111333)

Toggle Login With Facebook (boolean)

Facebook API Key (string)

Toggle Comment Album (boolean)

Toggle Comment Chapter (boolean)

Toggle Comment Use Disqus (boolean)

Disqus API Key (string)

Embedly API Key (string)

Toggle Email Use (mandrill,sendinblue,mailgun)

Mandrill API Key (string)

Sendinblue API Key (string)

Mailgun API Key (string)

Toggle Use TOTP Login (boolean)
```

# Models

## User

```
name string(256)
username string(128)
password string(50)
email string(128)
description string(1000)
tagline string(256)
profile_picture string(1024)
facebook_id string(64)
role enum(admin,user)
status enum(suspended,active)
```

## Collection

```
user_id ObjectId
name string(1024)
tags []string(256)
type enum(novel,comic)
```

## Node

```
collection_id ObjectId
name string(1024)
file_url string(1024)
file_type enum(pdf,image)
content string(16384)
```

## Comment

```
user_id ObjectId
target_id ObjectId
target_type enum(album,node)
content string(2048)
media {
	url string(1024)
	title string(1024)
	description string(2048)
	images []string(1024)
	type enum(video,images,opengraph)
	og_provider_display string(1024)
}
```

## Client

```
user_id ObjectId
name string(256)
logo string(1024)
description string(1024)
api_key string(32)
status enum(suspended,active)
```

## Response

```
error string
error_description string
data ANY
```
-------------------------------------------------------

# API Endpoints

## Meta

```
/meta/set (admin only)
Parameters:
Headers:
api-key (mandatory)
Response Data:

/meta/get/:key
Parameters:
key_name (admin only, except )
Headers:
api-key (mandatory)
Response Data:
```

## Comment

```
/comment
Parameters:
order_by (optional) (default: -created_at)
page (optional) (default: 0)
per_page (optional) (default: 10)
id (optional)
user_id (optional)
target_id (optional)
target_type (optional)
content (optional)
media_title (optional)
media_description (optional)
media_type (optional)
media_og_provider_display (optional)
Headers:
api-key (mandatory)
Response Data:
[]Comment

/comment/:id
Headers:
api-key (mandatory)
Response Data:
Comment

/comment/create
Parameters:
Comment Model properties (mandatory)
Headers:
api-key (mandatory)
Response Data:
Comment

/comment/update
Parameters:
Comment Model properties (mandatory)
Headers:
api-key (mandatory)
Response Data:
Comment

/comment/delete
Parameters:
id (mandatory)
Headers:
api-key (mandatory)
Response Data:
comment_id

```

## Client

```
/client (admin only)
Parameters:
order_by (optional) (default: -created_at)
page (optional) (default: 0)
per_page (optional) (default: 10)
id (optional)
name (optional)
user_id (optional)
description (optional)
status (optional)
Headers:
api-key (mandatory)
Response Data:
[]Client (omitting api_key)

/client/:id (admin and owner only)
Headers:
api-key (mandatory)
Response Data:
Client (omitting api_key for admin)

/client/create (only one time for users)
Parameters:
Client Model properties (mandatory) (omitting api_key)
Headers:
api-key (mandatory)
Response Data:
Client (omitting api_key for admin)

/client/update (admin and owner only)
Parameters:
Client Model properties (mandatory) (omitting api_key)
Headers:
api-key (mandatory)
Response Data:
Client (omitting api_key for admin)

/client/delete (admin and owner only)
Parameters:
id (mandatory)
Headers:
api-key (mandatory)
Response Data:
client_id

/client/suspend (admin only)
Parameters:
id (mandatory)
Headers:
api-key (mandatory)
Response Data:
client_id
```

## User

```
/login
Parameters:
username (mandatory)
password (mandatory)
Headers:
api-key (mandatory)
Response Data:
User

/logout
Headers:
api-key (mandatory)
Response Data:
user_id

/user
Parameters:
order_by (optional) (default: -created_at)
page (optional) (default: 0)
per_page (optional) (default: 10)
id (optional)
name (optional)
username (optional)
tagline (optional)
description (optional)
role (optional) (admin only)
status (optional) (admin only)
email (optional) (admin only)
facebook_id (optional) (admin only)
Headers: 
api-key (mandatory)
Response Data:
[]User (omitting password)

/user/me
Headers:
api-key (mandatory)
Response Data:
User (omitting password)

/user/:id
Headers:
api-key (mandatory)
Response Data:
User (omitting password)

/user/update (admin and owner only)
Parameters:
User Model Properties (mandatory) (omitting username except one time only)
Headers:
api-key (mandatory)
Response Data:
User (omitting password)

/user/create (admin only)
Parameters:
User Model Properties (mandatory)
Headers:
api-key (mandatory)
Response Data:
User (omitting password)

/user/delete (admin only)
Parameters:
id (mandatory)
Headers:
api-key (mandatory)
Response Data:
user_id

/user/suspend (admin only)
Parameters:
id (mandatory)
Headers:
api-key (mandatory)
Response Data:
user_id
```
## Oauth2

```
/oauth2/facebook/authorization
Parameters:
error_reason
error
error_description
code
state 
----
When you send request to facebook, you must attach state={Your Frontend Application ID} to the request. 

Then the server must verify this state callback.

After that verify the code to facebook.

/oauth2/facebook/token
Parameters:
access_token
token_type
expires_in
---
After receiving response from facebook, we send request to facebook using GET like this:
https://graph.facebook.com/debug_token?input_token={code}&access_token={access_token}

Then we should verify if the returned user_id, app_id, expires_at, and is_valid is okay.

After that, request user profile data to /user/me?access_token={access_token} to get and upsert user's data in our server (by using id in response as facebook_id in our database.)

Then generate a session with user_id, access_token, and role in it before sending response to the client.

Upsert, if insert operation should generate random username that the user could change one time only with a time limit until rendered permanent.
```