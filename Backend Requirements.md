# Backend Requirements
Backend is only an API service without any rendering, and just focus on producing JSON for client consumption. This document is still a draft and will be frequently updated.

# Services
These are the list of services used in a Cowmilk server and at least one from each categories must be supported:

- Login
    - Facebook

- Email
    - Mandrill
    - Sendinblue
    - Mailgun
    - Sendgrid

- Embedded Media
    - Embedly

- Comment
    - Disqus

# SLA
These are the minimum specification a Cowmilk server has and must be upheld at all times.

- Minimum Uptime Percentage
    - TBA

- Maximum response time
    - TBA

------------------------------------------------------

# Metas

#### Logo 
The URL to logo image of the Cowmilk server.

##### Key
`logo`

##### Example
`http://your_url/example.jpeg`

#### Background
The URL to background image of the Cowmilk server.

##### Key
`background`

##### Example
`http://your_url/example.jpeg`

#### Theme
The theme name of the Cowmilk server, used by frontend to determine the correct theme.

##### Key
`theme`

##### Example
`luna`

#### Color Scheme
The color scheme for the frontend, coded in hexadecimal representation of colors.
Could be more than one item separated by a comma.

##### Key
`color-scheme`

##### Example
`#aaabbb,#ccceee,#111333`

##### Toggle Album Comment
An option to toggle album comment. Will reject any update or insert into API too.

##### Key
`toggle-album-comment`

##### Example
`true`

#### Toggle Chapter Comment
An option to toggle chapter comment. Will reject any update or insert into API too.

##### Key
`toggle-chapter-comment`

##### Example
`true`

##### Third Party Login
The third party login service(s) that must be showed on the frontend and enabled on the backend.
Can be more than one item separated by a comma.

##### Key
`third-party-login`

##### Example
`facebook,twitter`

#### Email Service
The email service(s) used by the Cowmilk server for sending email, mostly account verification mail.

##### Key
`email-service`

##### Example
`sendgrid`

#### Comment Service
The comment service used by the Cowmilk server.

##### Key
`comment-service`

##### Example
`disqus`

#### Embedded Media Service
The third party embedded media service that is used by the Cowmilk server.

##### Key
`embedded-media-service`

##### Example
`embedly`

#### API Key
API key for the services.

##### Key
`SERVICE_NAME-api-key`

##### Example
`akeok931m3bfm3i27113`

# Models

A Cowmilk server must have at least these informations in their database(s). 
The actual structure of the model in the database and implementation are totally in the developer's hands.
The format is `PROPERTY_NAME PROPERTY_TYPE(MAX_MIN_OR_POSSIBLE_VALUES)`.

## User

```
name string(256)
username string(128)
password string(50)
email string(128)
description string(1000)
tagline string(256)
profile_picture string(1024)
role enum(admin,user)
status enum(suspended,active)
```

## Collection

```
user_id ObjectId
name string(1024)
tags []string(256)
type enum(novel,comic)
commentable bool
```

## Node

```
collection_id ObjectId
name string(1024)
file_url string(1024)
file_type enum(pdf,image)
content string(16384)
commentable bool
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

# API

## Meta

The server must have a pair of API endpoints to get and set meta data.

#### /meta/set **POST
Only admin have access to this endpoint.

##### Parameters:

##### Headers:
- api-key **(mandatory)

##### Response Data:

#### /meta/get/:key **GET

##### Parameters:
- key_name **(admin only, except some data that deemed necessary for the frontend)

##### Headers:
- api-key **(mandatory)

##### Response Data:

## Oauth2

The server should support login via third party services, like facebook, twitter, etc. 
The convention for the routes must be `/oauth/VERSION/VENDOR/authorization` to receive authorization code,
and `/oauth/VERSION/VENDOR/token` to receive access token.

How to process and implement are fully given to the server developer for the best practice.

Next is the example of facebook oauth2 authorization for a Cowmilk server.

#### /oauth/2/facebook/authorization **GET

##### Parameters:
- error_reason
- error
- error_description
- code
- state 

When you send request to facebook, you must attach `state={Your Frontend Application ID}` to the request. 

Then the server must verify this state callback.

After that verify the code to facebook.

#### /oauth/2/facebook/token **POST

##### Parameters:
- access_token
- token_type
- expires_in

After receiving response from facebook, we send request to facebook using GET like this:
`https://graph.facebook.com/debug_token?input_token={code}&access_token={access_token}`

Then we should verify if the returned `user_id`, `app_id`, `expires_at`, and `is_valid` is valid.

After that, request user profile data to facebook's `/user/me?access_token={access_token}` to get and upsert user's data in our server (by using id in response as `facebook_id` property in our database.)

Then generate a session with `user_id`, `access_token`, and `role` in it before sending response to the client.

## GraphQL

A Cowmilk server must support [GraphQL](https://facebook.github.io/graphql) for both fetching and upserting data.

##### /data **GET

##### /data **POST