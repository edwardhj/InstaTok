# TravelTok

## Database Schema Design

![db-schema]

[db-schema]: ./database_schema.png

## TravelTok API Documentation

Listed below are all the endpoints of the backend for TravelTok

## User Authentication/Authorization

### All endpoints that require authentication

All endpoints that require a current user to be logged in.

- Request: endpoints that require authentication
- Error Response: Require authentication

  - Status Code: 401
  - Headers:
    - Content-Type: application/json
  - Body:

  ```json
  {
  	"message": "Authentication required"
  }
  ```

### All endpoints that require proper authorization

All endpoints that require authentication and the current user does not have the correct role(s) or permission(s).

- Request: endpoints that require proper authorization
- Error Response: Require proper authorization

  - Status Code: 403
  - Headers:
    - Content-Type: application/json
  - Body:

    ```json
    {
    	"message": "Forbidden"
    }
    ```

### Get the Current User

Returns the information about the current user that is logged in.

- Require Authentication: false
- Request

  - Method: GET
  - URL: /api/auth
  - Body: none

- Successful Response when there is a logged in user

  - Status Code: 200
  - Headers:
    - Content-Type: application/json
  - Body:

    ```json
    {
    	"id": 1,
    	"first_name": "John",
      "last_name": "Smith",
    	"email": "johnsmith@gmail.com",
      "username": "thejohnsmith",
      "isAdmin": false,
      "profile_pic": "aws_link_to_profile_pic"
    }
    ```

* Error Response when there is no logged in user

  - Status Code: 401
  - Headers:
    - Content-Type: application/json
  - Body:

    ```json
    {
    	"error": "No user is logged in"
    }
    ```

### Log In a User

Logs in a current user with valid credentials and returns the current user's
information.

- Require Authentication: false
- Request

  - Method: POST
  - URL: /api/auth/login
  - Headers:
    - Content-Type: application/json
  - Body:

    ```json
    {
    	"email": "johnsmith@gmail.com",
    	"password": "password"
    }
    ```

* Successful Response

  - Status Code: 200
  - Headers:
    - Content-Type: application/json
  - Body:

    ```json
    {
    	"id": 1,
    	"first_name": "John",
      "last_name": "Smith",
    	"email": "johnsmith@gmail.com",
      "username": "thejohnsmith",
      "isAdmin": false,
      "profile_pic": "aws_link_to_profile_pic"
    }
    ```

* Error Response: Invalid credentials

  - Status Code: 401
  - Headers:
    - Content-Type: application/json
  - Body:

    ```json
    {
    	"message": "Invalid credentials"
    }
    ```

* Error response: Body validation errors

  - Status Code: 400
  - Headers:
    - Content-Type: application/json
  - Body:

    ```json
    {
    	"message": "Bad Request", // (or "Validation error" if generated by wtforms),
    	"errors": {
    		"email": ["This field is required"],
    		"password": ["This field is required"]
    	}
    }
    ```

### Log Out a User

Logs out a current user

- Require Authentication: true
- Request

  - Method: POST
  - URL: /api/auth/logout
  - Headers:
    - Content-Type: application/json
  - Body: none

* Successful Response

  - Status Code: 200
  - Headers:
    - Content-Type: application/json
  - Body:

    ```json
    {
    	"message": "User has been successfully logged out"
    }
    ```    

### Sign Up a User

Creates a new user, logs them in as the current user, and returns the current
user's information.

- Require Authentication: false
- Request

  - Method: POST
  - URL: /api/auth/signup
  - Headers:
    - Content-Type: application/json
  - Body:

    ```json
    {
    	"first_name": "John",
      "last_name": "Smith",
    	"email": "johnsmith@gmail.com",
      "username": "thejohnsmith",
      "password": "password",
      "isAdmin": false,
      "profile_pic": "profile picture file"
    }
    ```

* Successful Response

  - Status Code: 200
  - Headers:
    - Content-Type: application/json
  - Body:

    ```json
    {
    	"id": 1,
    	"first_name": "John",
      "last_name": "Smith",
    	"email": "johnsmith@gmail.com",
      "username": "thejohnsmith",
      "isAdmin": false,
      "profile_pic": "aws_link_to_profile_pic"
    }
    ```

* Error response: User already exists with the specified email

  - Status Code: 500
  - Headers:
    - Content-Type: application/json
  - Body:

    ```json
    {
    	"message": "User already exists",
    	"errors": {
    		"email": "Email address is already in use"
    	}
    }
    ```

* Error response: User already exists with the specified username

  - Status Code: 500
  - Headers:
    - Content-Type: application/json
  - Body:

    ```json
    {
    	"message": "User already exists",
    	"errors": {
    		"username": "Username is already in use"
    	}
    }
    ```

* Error response: Body validation errors

  - Status Code: 400
  - Headers:
    - Content-Type: application/json
  - Body:
    ```json
    {
    	"message": "Bad Request", // (or "Validation error" if generated by wtforms),
    	"errors": {
    		"email": "Invalid email",
    		"first_name": "First Name is required",
        "last_name": "Last Name is required",
        "password": "Password is required"
    	}
    }
    ```

## CLIPS

### Get all Clips

Returns all the clips that are public (is not private)

- Request
  - Method: GET
  - URL: /api/clips
  - Body: none

- Successful Response
  - Status Code: 200
  - Headers:
    - Content-Type: application/json
  - Body:

  ```json
  {
  	"Clips": [
      {
        "id": 1,
        "user_id": 1,
        "location": "Paris",
        "video_file": "aws video link",
        "caption": "Paris in Spring",
        "creator": "demolition",
        "created_at": "2024-04-20 18:20:00",
        "updated_at": "2024-04-20 18:20:00"
      }
    ]
  }
  ```

### Get all Clips owned by Current User

Returns all the clips that are created by the current user

- Request
  - Method: GET
  - URL: /api/clips/current
  - Body: none

- Successful Response
  - Status Code: 200
  - Headers:
    - Content-Type: application/json
  - Body:

  ```json
  [
    {
      "id": 1,
      "user_id": 1,
      "location": "Paris",
      "video_file": "aws video link",
      "caption": "Paris in Spring",
      "is_private": false,
      "created_at": "2024-04-20 18:20:00",
      "updated_at": "2024-04-20 18:20:00"
    }
  ]
  ```

### Get details of a Clip from a Clip ID

Returns the details of a clip specified by its id

- Require Authorization: If clip is private, it must belong to logged in user

- Request
  - Method: GET
  - URL: /api/clips/:clipId
  - Body: none

- Successful Response
  - Status Code: 200
  - Headers:
    - Content-Type: application/json
  - Body:

  ```json

  {
    "id": 1,
    "user_id": 1,
    "location": "Paris",
    "video_file": "aws video link",
    "caption": "Paris in Spring",
    "creator": "Demolition",
    "is_private": false,
    "created_at": "2024-04-20 18:20:00",
    "updated_at": "2024-04-20 18:20:00",
    "comments": ["array of comments"]
  }

  ```

* Error response: Couldn't find a clip with the specified id

- Status Code: 404
- Headers:
- Content-Type: application/json
- Body:

  ```json
    "message": "Clip couldn't be found"
  ```

### Create a Clip

Creates and returns a new clip

- Require Authentication: true

- Request
  - Method: POST
  - URL: /api/clips/new
  - Body:

```json
  {
      {
        "location": "Paris",
        "video_file": "video file",
        "caption": "Paris in Spring",
        "is_private": false
      }
  }
  ```

- Successful Response
  - Status Code: 201
  - Headers:
    - Content-Type: application/json
  - Body:

  ```json
  {
      {
        "id": 1,
        "user_id": 1,
        "location": "Paris",
        "video_file": "aws video link",
        "caption": "Paris in Spring",
        "is_private": false,
        "created_at": "2024-04-20 18:20:00"
      }
  }
  ```

* Error response: Body validation errors

- Status Code: 400
- Headers:
- Content-Type: application/json
- Body:

  ```json
    {
      "message": "Bad Request", // or "validation error" if generated by WTForms,
      "errors": {
        "location": "Location is required",
        "video_file": "A video must be uploaded"
      }
    }
  ```

### Edit a Clip

Updates and returns an existing clip

- Require Authentication: true
- Require Authorization: Clip must belong to logged in user

- Request
  - Method: PUT
  - URL: /api/clips/:clipId
  - Body:

```json
  {
      {
        "location": "London",
        "caption": "London in Spring",
        "is_private": false
      }
  }
  ```

- Successful Response
  - Status Code: 200
  - Headers:
    - Content-Type: application/json
  - Body:

  ```json
  {
      {
        "id": 1,
        "user_id": 1,
        "location": "London",
        "video_file": "aws video link",
        "caption": "London in Spring",
        "is_private": false,
        "created_at": "2024-04-20 18:20:00"
      }
  }
  ```

* Error response: Body validation errors

- Status Code: 400
- Headers:
- Content-Type: application/json
- Body:

  ```json
    {
      "message": "Bad Request", // or "validation error" if generated by WTForms,
      "errors": {
        "location": "Location is required"
      }
    }
  ```

* Error response: Couldn't find a Clip with the specified id

- Status Code: 404
- Headers:
- Content-Type: application/json
- Body:

  ```json
    {
      "message": "Clip couldn't be found"
    }
  ```

### Delete a Clip

Deletes an existing Clip

- Require Authentication: true
- Require Authorization: Clip must belong to logged in user

- Request
  - Method: DELETE
  - URL: /api/clips/:clipId
  - Body: none

- Successful Response
  - Status Code: 200
  - Headers:
    - Content-Type: application/json
  - Body:

  ```json
  {
    "message": "Clip has been successfully deleted"
  }
  ```

* Error response: Couldn't find a Clip with the specified id

- Status Code: 404
- Headers:
- Content-Type: application/json
- Body:

  ```json
    {
      "message": "Clip couldn't be found"
    }
  ```

## COMMENTS

### Get all Comments of the Logged In User

Returns all the comments that are written by the logged in user

- Require Authentication: true

- Request
  - Method: GET
  - URL: /api/comments/current
  - Body: none

- Successful Response
  - Status Code: 200
  - Headers:
    - Content-Type: application/json
  - Body:

  ```json
  [
      {
        "id": 1,
        "user_id": 1,
        "clip_id": 1,
        "body": "Awesome Video! Didn't know Paris had such a beautiful river",
        "created_at": "2024-04-20 18:20:00"
      }
  ]
  ```

### Get all Comments by Clip, specified by Clip id

Returns all the comments that belong to a clip post specified by clip id
(this will now be retrievable underneath the get a clip by clipId route & the route listed below has been retired)

- Request
  - Method: GET
  - URL: /api/clips/:clipId/comments
  - Body: none

- Successful Response
  - Status Code: 200
  - Headers:
    - Content-Type: application/json
  - Body:

  ```json
  {
  	"Comments": [
      {
        "id": 1,
        "user_id": 1,
        "clip_id": 1,
        "body": "Awesome Video! Didn't know Paris had such a beautiful river",
        "created_at": "2024-04-20 18:20:00",
        "User": {
          "id": 1,
          "username": "thejohnsmith"
        }
      }
    ]
  }
  ```

* Error response: Couldn't find a clip with the specified id

- Status Code: 404
- Headers:
- Content-Type: application/json
- Body:

  ```json
  {
    "message": "Clip couldn't be found"
  }
  ```

### Create a Comment for a Clip based on the Clip's id

Create and return a new comment for a clip, specified by clip id

- Require Authentication: true

- Request
  - Method: POST
  - URL: /api/clips/:clipId/comments
  - Body:
  
  ```json
  {
    "body": "London is quite beautiful in the summer"
  }
  ```

- Successful Response
  - Status Code: 201
  - Headers:
    - Content-Type: application/json
  - Body:

```json
{
  "message": "Comment successfully created."
}
```

* Error response: Body validation errors

- Status Code: 400
- Headers:
- Content-Type: application/json
- Body:

  ```json
  {
    "message": "Bad Request", // or "validation error" if generated by WTForms
    "errors": {
      "body": "Comment text is required"
    }
  }
  ```

* Error response: Couldn't find a clip with the specified id

- Status Code: 404
- Headers:
- Content-Type: application/json
- Body:

  ```json
  {
    "message": "Clip couldn't be found"
  }
  ```

### Edit a Comment

Update and return an existing comment

- Require Authentication: true
- Require Authorization: Comment must belong to logged in user

- Request
  - Method: PUT
  - URL: /api/comments/:commentId
  - Body:

  ```json
  {
    "body": "London is not beautiful in the winter"
  }
  ```

- Successful Response
  - Status Code: 200
  - Headers:
    - Content-Type: application/json
  - Body:

```json
{
  "message": "comment has been updated successfully"
}
```

* Error response: Body validation errors

- Status Code: 400
- Headers:
- Content-Type: application/json
- Body:

  ```json
  {
    "message": "Bad Request", // or "validation error" if generated by WTForms
    "errors": {
      "body": "Comment text is required"
    }
  }
  ```

### Delete a Comment

Deletes an existing Comment

- Require Authentication: true
- Require Authorization: Comment must belong to logged in user

- Request
  - Method: DELETE
  - URL: /api/comments/:commentId
  - Body: none

- Successful Response
  - Status Code: 200
  - Headers:
    - Content-Type: application/json
  - Body:

  ```json
  {
    "message": "Comment has been successfully deleted"
  }
  ```

* Error response: Couldn't find a Comment with the specified id

- Status Code: 404
- Headers:
- Content-Type: application/json
- Body:

  ```json
    {
      "message": "Comment couldn't be found"
    }
  ```


## Likes

### Get all liked Clips by logged in user

Returns all clips liked by the logged in user

- Require Authentication: true
- Require Authorization: Likes must belong to logged in user

* Request:
  - Method: GET
  - URL: /api/likes/current
  - Headers:
    - Content-Type: application/json
  - Body: None

* Successful Response:
  - Status Code: 200
  - Headers:
    - Content-Type: application/json
  - Body:

  ```json
  {
    "Clips": [
      {
        "id": 1,
        "user_id": 1,
        "location": "Paris",
        "video_file": "aws video link",
        "caption": "Paris in Spring",
        "created_at": "2024-04-20 18:20:00"
      }
    ]
  }
  ```

### Create a like

Creates and returns a like

- Require Authentication: true
- Require Authorization: false

* Request:
  - Method: POST
  - URL: /api/clips/:clipId/likes
  - Headers:
    - Content-Type: application/json
  - Body:
  ```json
  {
    "clip_id": 1,
    "is_like": true
  }
  ```
* Successful Response:
  - Status Code: 201
  - Headers:
    - Content-Type: application/json
  - Body:
  ```json
  {
  	"message": "You have successfully liked the Clip"
  }
  ```

* Error Response:
  - Status Code: 404
  - Headers:
    - Content-Type: application/json
  - Body:

```json
{
	"message": "Clip couldn't be found"
}
```

### Edit a like

Updates and returns an existing like, turned into a dislike

- Require Authentication: true
- Require Authorization: like must be created by logged in user

* Request:
  - Method: PUT
  - URL: /api/clips/:clipId/likes
  - Headers:
    - Content-Type: application/json
  - Body:
  ```json
  {
    "clip_id": 1,
    "is_like": false
  }
  ```
* Successful Response:
  - Status Code: 200
  - Headers:
    - Content-Type: application/json
  - Body:
  ```json
  {
  	"message": "You have successfully disliked the Clip"
  }
  ```

* Error Response:
  - Status Code: 404
  - Headers:
    - Content-Type: application/json
  - Body:

```json
{
	"message": "Clip couldn't be found"
}
```

### Delete a like
Deletes a like for a Clip

- Require Authentication: true
- Require Authorization: Deleted like must be owned by logged in user

* Request:
  - Method: DELETE
  - URL: /api/clips/:clipId/likes
  - Headers:
    - Content-Type: application/json
  - Body: None

* Successful Response:
  - Status Code: 200
  - Headers:
    - Content-Type: application/json
```json
{
	"message": "Successfully Deleted Like"
}
```

* Error Response:
  - Status Code: 404
  - Headers:
    - Content-Type: application/json
  - Body:

```json
{
	"message": "Like couldn't be found"
}
```

## Follows

### Get all followers by user_id

Returns a count of all the followers a user has

- Require Authentication: false
- Require Authorization: false

* Request:
  - Method: GET
  - URL: /api/follows/followers/:userId
  - Body: None

* Successful Response:
  - Status Code: 200
  - Headers:
    - Content-Type: application/json
  - Body:

  ```json
  {
      {
        "followers": <number of followers>
      }
  }
  ```

### Get all following by user_id

Returns a count of all the users a user follows

- Require Authentication: false
- Require Authorization: false

* Request:
  - Method: GET
  - URL: /api/follows/following/current
  - Body: None

* Successful Response:
  - Status Code: 200
  - Headers:
    - Content-Type: application/json
  - Body:

  ```json
  {
      {
        "followers": <number of following>
      }
  }
  ```

### Create a follow

Creates and returns a follow

- Require Authentication: true
- Require Authorization: false

* Request:
  - Method: POST
  - URL: /api/follows/following/:userId
  - Body:

  ```json
  {
    "following_user_id": 1
  }
  ```

* Successful Response:
  - Status Code: 201
  - Headers:
    - Content-Type: application/json
  - Body:

  ```json
  {
      {
        "message": "You have successfully followed <user>"
      }
  }
  ```

* Error Response: You already follow the user
  - Status Code: 400
  - Headers:
    - Content-Type: application/json
  - Body:

```json
{
	"message": "You are already following the user"
}
```

* Error Response: User does not exist
  - Status Code: 400
  - Headers:
    - Content-Type: application/json
  - Body:

```json
{
	"message": "User could not be found"
}
```

### Update a user you follow to a close friend

Updates and returns an existing user you follow to a close friend

- Require Authentication: true
- Require Authorization: follow must be created by other user

* Request:
  - Method: PUT
  - URL: /api/follows/following/:userId
  - Body:

  ```json
  {
    "follower_user_id": 1,
    "is_close_friend": true
  }
  ```

* Successful Response:
  - Status Code: 200
  - Headers:
    - Content-Type: application/json
  - Body:

  ```json
  {
      {
        "message": "You have successfully added <user> to your close friends list"
      }
  }
  ```

* Error Response: You do not follow the user
  - Status Code: 400
  - Headers:
    - Content-Type: application/json
  - Body:

```json
{
	"message": "You do not follow the user"
}
```

* Error Response: User is already a close friend
  - Status Code: 400
  - Headers:
    - Content-Type: application/json
  - Body:

```json
{
	"message": "User is already a close friend"
}
```

* Error Response: User does not exist
  - Status Code: 400
  - Headers:
    - Content-Type: application/json
  - Body:

```json
{
	"message": "User could not be found"
}
```

### Delete a follow

Deletes an existing follow

- Require Authentication: true
- Require Authorization: follow must be created by logged in user

* Request:
  - Method: DELETE
  - URL: /api/follows/following/:userId
  - Body:

  ```json
  {
    "following_user_id": 1
  }
  ```

* Successful Response:
  - Status Code: 200
  - Headers:
    - Content-Type: application/json
  - Body:

  ```json
  {
      {
        "message": "You have successfully unfollowed <user>"
      }
  }
  ```

* Error Response: You are not following the user
  - Status Code: 400
  - Headers:
    - Content-Type: application/json
  - Body:

```json
{
	"message": "You do not follow the user"
}
```

* Error Response: User does not exist
  - Status Code: 400
  - Headers:
    - Content-Type: application/json
  - Body:

```json
{
	"message": "User could not be found"
}
```

