# Friendship API

The Friendship API allows users to create, manage, and remove friendships between players in the GameFuse platform. This includes sending friend requests, accepting or rejecting them, unfriending players, and viewing friendship data.

---

## Sending a Friend Request

### Scope

Create a new friendship request by providing the `username` of the user you want to add as a friend.

### Method

!!! info annotate "POST"
	```plaintext
	/api/v3/friendships
	```

### Attributes

| Name       | Type   | Required | Description |
|------------|--------|----------|-------------|
| `username` | string | Yes      | The username of the player you want to send a friend request to. |

### Headers

| Name                  | Type   | Description |
|-----------------------|--------|-------------|
| `authentication-token` | string | Found in sign-in or sign-up responses. This token is used for user sessions |
| `Content-Type`         | string | Set it to `application/json` |

### Responses

| HTTP status code | content-type    | Description                         |
|------------------|-----------------|-------------------------------------|
| `200`            | application/json| Friendship request sent successfully. |
| `422`            | text/plain      | Username is missing or user does not exist. |
| `500`            | text/plain      | Unknown server error.               |

### Response object

| Attribute name | Type   | Description                             |
|----------------|--------|-----------------------------------------|
| `message`      | string | Success message confirming the friend request. |
| `friendship_id`| integer| The ID of the created friendship request. |

### Examples

!!! example
	#### cURL

	```shell
	curl --request POST \
		--header "authentication-token: abc123" \
		--header "Content-Type: application/json" \
		--data '{"username": "friend_username"}' \
		"https://gamefuse.co/api/v3/friendships"
	```

	#### Response

	```json
	{
	  "message": "Friend request sent to friend_username",
	  "friendship_id": 123
	}
	```

---

## Accept or Decline a Friend Request

### Scope

Accept or decline a pending friendship request by specifying the `status` parameter.

### Method

!!! info annotate "PUT"
	```plaintext
	/api/v3/friendships/{id}
	```

### Attributes

| Name       | Type   | Required | Description                                |
|------------|--------|----------|--------------------------------------------|
| `id`       | integer| Yes      | The friendship request ID.                 |
| `status`   | string | Yes      | The new status of the friendship: `accepted` or `declined`. |

### Headers

| Name                  | Type   | Description |
|-----------------------|--------|-------------|
| `authentication-token` | string | Found in sign-in or sign-up responses. This token is used for user sessions |
| `Content-Type`         | string | Set it to `application/json` |

### Responses

| HTTP status code | content-type    | Description                                      |
|------------------|-----------------|--------------------------------------------------|
| `200`            | application/json| Success message indicating the new status of the friendship. |
| `422`            | text/plain      | Invalid status or unauthorized to accept this friendship. |
| `500`            | text/plain      | Unknown server error.                            |

### Response object

| Attribute name | Type   | Description                                  |
|----------------|--------|----------------------------------------------|
| `message`      | string | Message confirming the friendship status update. |

### Examples

!!! example
	#### cURL

	```shell
	curl --request PUT \
		--header "authentication-token: abc123" \
		--header "Content-Type: application/json" \
		--data '{"status": "accepted"}' \
		"https://gamefuse.co/api/v3/friendships/1"
	```

	#### Response

	```json
	{
	  "message": "You have successfully accepted this friend request"
	}
	```

---

## Cancel a Friend Request

### Scope

Cancel a pending friend request that was previously sent.

### Method

!!! info annotate "DELETE"
	```plaintext
	/api/v3/friendships/{id}
	```

### Attributes

| Name | Type    | Required | Description                   |
|------|---------|----------|-------------------------------|
| `id` | integer | Yes      | The friendship request ID.     |

### Headers

| Name                  | Type   | Description |
|-----------------------|--------|-------------|
| `authentication-token` | string | Found in sign-in or sign-up responses. This token is used for user sessions |
| `Content-Type`         | string | Set it to `application/json` |

### Responses

| HTTP status code | content-type    | Description                                      |
|------------------|-----------------|--------------------------------------------------|
| `200`            | application/json| Success message indicating the friend request has been cancelled. |
| `422`            | text/plain      | Unauthorized or invalid friendship ID.           |
| `500`            | text/plain      | Unknown server error.                            |

### Response object

| Attribute name | Type   | Description                            |
|----------------|--------|----------------------------------------|
| `message`      | string | Message confirming the cancellation of the friend request. |

### Examples

!!! example
	#### cURL

	```shell
	curl --request DELETE \
		--header "authentication-token: abc123" \
		"https://gamefuse.co/api/v3/friendships/1"
	```

	#### Response

	```json
	{
	  "message": "Friend request destroyed successfully"
	}
	```

---

## Unfriend a Player

### Scope

Remove an accepted friend from the current user's friend list.

### Method

!!! info annotate "DELETE"
	```plaintext
	/api/v3/unfriend?user_id={friendUserId}
	```

### Attributes

| Name         | Type    | Required | Description                                |
|--------------|---------|----------|--------------------------------------------|
| `user_id`    | integer | Yes      | The ID of the user you want to unfriend.   |

### Headers

| Name                  | Type   | Description |
|-----------------------|--------|-------------|
| `authentication-token` | string | Found in sign-in or sign-up responses. This token is used for user sessions |
| `Content-Type`         | string | Set it to `application/json` |

### Responses

| HTTP status code | content-type    | Description                               |
|------------------|-----------------|-------------------------------------------|
| `200`            | application/json| Success message indicating the user has been unfriended. |
| `422`            | text/plain      | Friendship does not exist or invalid user ID. |
| `500`            | text/plain      | Unknown server error.                     |

### Response object

| Attribute name | Type   | Description                             |
|----------------|--------|-----------------------------------------|
| `message`      | string | Message confirming the user was unfriended. |

### Examples

!!! example
	#### cURL

	```shell
	curl --request DELETE \
		--header "authentication-token: abc123" \
		"https://gamefuse.co/api/v3/unfriend?user_id=1"
	```

	#### Response

	```json
	{
	  "message": "User has been unfriended successfully"
	}
	```

---

## Get Friendship Data

### Scope

Retrieve the list of friends, outgoing, and incoming friendship requests for the current user.

### Method

!!! info annotate "GET"
	```plaintext
	/api/v3/friendships
	```

### Attributes

None

### Headers

| Name                  | Type   | Description |
|-----------------------|--------|-------------|
| `authentication-token` | string | Found in sign-in or sign-up responses. This token is used for user sessions |

### Responses

| HTTP status code | content-type    | Description                               |
|------------------|-----------------|-------------------------------------------|
| `200`            | application/json| Object containing friends, outgoing requests, and incoming friend requests. |
| `500`            | text/plain      | Unknown server error.                     |

### Response object

| Attribute name              | Type   | Description                             |
|-----------------------------|--------|-----------------------------------------|
| `friends`                    | array  | List of friends (basic user info).      |
| `outgoing_friend_requests`   | array  | List of friend requests sent by the user. |
| `incoming_friend_requests`   | array  | List of friend requests received by the user. |

### Examples

!!! example
	#### cURL

	```shell
	curl --request GET \
		--header "authentication-token: abc123" \
		"https://gamefuse.co/api/v3/friendships"
	```

	#### Response

	```json
	{
	  "friends": [
		{
		  "id": 1,
		  "username": "john_doe",
		  "email": "john.doe@example.com",
		  "display_email": "john.doe@example.com",
		  "credits": 150,
		  "score": 1200
		}
	  ],
	  "outgoing_friend_requests": [
		{
		  "id": 2,
		  "username": "friend_username",
		  "email": "friend@example.com",
		  "display_email": "friend@example.com",
		  "credits": 100,
		  "score": 900,
		  "friendship_id": 123,
		  "requested_at": "2024-09-20T12:34:56Z"
		}
	  ],
	  "incoming_friend_requests": [
		{
		  "id": 3,
		  "username": "another_friend",
		  "email": "another_friend@example.com",
		  "display_email": "another_friend@example.com",
		  "credits": 200,
		  "score": 1300,
		  "friendship_id": 124,
		  "requested_at": "2024-09-19T11:22:33Z"
		}
	  ]
	}
	```
