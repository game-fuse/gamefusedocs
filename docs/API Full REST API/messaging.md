# Chat and Message API

The Chat and Message API allows users to manage chats and send messages between players in the GameFuse platform. This includes creating chats, sending messages, paginating through chats and messages, and marking messages as read.

!!! note
    This feature is not supported in the client libraries yet.  It is in beta and may be subject to change. This message will be removed when the feature is completed
	
---

## Chats

### Fetch Paginated Chats

### Scope

Retrieve the paginated list of chats (both direct and group chats) the user is a part of.

### Method

!!! info annotate "GET"
	```plaintext
	/api/v3/chats/page/{page}
	```

### Attributes

| Name  | Type   | Required | Description                  |
|-------|--------|----------|------------------------------|
| `page` | integer | No       | Page number for pagination. If not provided, defaults to page 1. |

### Headers

| Name                  | Type   | Description |
|-----------------------|--------|-------------|
| `authentication-token` | string | Found in sign-in or sign-up responses. This token is used for user sessions |

### Responses

| HTTP status code | content-type    | Description                         |
|------------------|-----------------|-------------------------------------|
| `200`            | application/json| List of direct and group chats.     |
| `500`            | text/plain      | Unknown server error.               |

### Response object

| Attribute name | Type   | Description                   |
|----------------|--------|-------------------------------|
| `direct_chats` | array  | List of direct chats.          |
| `group_chats`  | array  | List of group chats.           |

#### Chat Object Structure

Each chat contains:

| Attribute name | Type   | Description                   |
|----------------|--------|-------------------------------|
| `id`           | integer| Chat ID                        |
| `creator_id`   | integer| ID of the user who created the chat |
| `creator_type` | string | The type of creator (`User`, etc.) |
| `messages`     | array  | List of most recent messages in the chat |
| `participants` | array  | List of users in the chat      |

#### Message Object Structure (in Chat Object)

Each message contains:

| Attribute name | Type    | Description                           |
|----------------|---------|---------------------------------------|
| `id`           | integer | Message ID                            |
| `text`         | string  | Message text                          |
| `user_id`      | integer | ID of the user who sent the message   |
| `created_at`   | string  | Timestamp when the message was created |
| `read_by`      | array   | List of user IDs who have read the message |
| `read`         | boolean | Whether the current user has read the message |

### Examples

!!! example
	#### cURL

	```shell
	curl --request GET \
		--header "authentication-token: abc123" \
		"https://gamefuse.co/api/v3/chats/page/1"
	```

	#### Response

	```json
	{
	  "direct_chats": [
		{
		  "id": 1,
		  "creator_id": 10,
		  "creator_type": "User",
		  "messages": [
			{
			  "id": 5,
			  "text": "Hey there!",
			  "user_id": 10,
			  "created_at": "2024-09-20T12:00:00Z",
			  "read_by": [10, 11],
			  "read": true
			}
		  ],
		  "participants": [
			{
			  "id": 10,
			  "username": "john_doe",
			  "email": "john.doe@example.com",
			  "display_email": "john.doe@example.com",
			  "credits": 150,
			  "score": 1200
			},
			{
			  "id": 11,
			  "username": "jane_smith",
			  "email": "jane.smith@example.com",
			  "display_email": "jane.smith@example.com",
			  "credits": 200,
			  "score": 1300
			}
		  ]
		}
	  ],
	  "group_chats": []
	}
	```

---

## Create a Chat

### Scope

Create a new chat, either a direct chat or a group chat, and send the first message.

### Method

!!! info annotate "POST"
	```plaintext
	/api/v3/chats
	```

### Attributes

| Name        | Type    | Required | Description                                         |
|-------------|---------|----------|-----------------------------------------------------|
| `usernames` | array   | No       | List of usernames to start a direct chat with.      |
| `group_id`  | integer | No       | ID of the group to send a message to (for group chat). |
| `text`      | string  | Yes      | The message text to send when creating the chat.    |

### Headers

| Name                  | Type   | Description |
|-----------------------|--------|-------------|
| `authentication-token` | string | Found in sign-in or sign-up responses. This token is used for user sessions |
| `Content-Type`         | string | Set it to `application/json` |

### Responses

| HTTP status code | content-type    | Description                                      |
|------------------|-----------------|--------------------------------------------------|
| `200`            | application/json| Chat created successfully and message sent.      |
| `422`            | text/plain      | Invalid parameters or missing required attributes. |
| `500`            | text/plain      | Unknown server error.                            |

### Response object

| Attribute name | Type   | Description                    |
|----------------|--------|--------------------------------|
| `chat`         | object | The newly created chat object.  |
| `chat_users`   | array  | List of users in the chat.      |

### Examples

!!! example
	#### cURL

	```shell
	curl --request POST \
		--header "authentication-token: abc123" \
		--header "Content-Type: application/json" \
		--data '{"usernames": ["user1", "user2"], "text": "Hello, everyone!"}' \
		"https://gamefuse.co/api/v3/chats"
	```

	#### Response

	```json
	{
	  "chat": {
		"id": 123,
		"creator_id": 10,
		"creator_type": "User",
		"messages": [
		  {
			"id": 5,
			"text": "Hello, everyone!",
			"user_id": 10,
			"created_at": "2024-09-20T12:34:56Z",
			"read_by": [10, 11],
			"read": true
		  }
		],
		"participants": [...]
	  },
	  "chat_users": [...]
	}
	```

---

## Messages

## Fetch Paginated Messages

### Scope

Retrieve the paginated list of messages for a specific chat.

### Method

!!! info annotate "GET"
	```plaintext
	/api/v3/messages/page/{page}?chat_id={chat_id}
	```

### Attributes

| Name     | Type    | Required | Description                             |
|----------|---------|----------|-----------------------------------------|
| `page`   | integer | No       | Page number for pagination. Defaults to page 1 if not provided. |
| `chat_id`| integer | Yes      | The chat ID to fetch messages for.      |

### Headers

| Name                  | Type   | Description |
|-----------------------|--------|-------------|
| `authentication-token` | string | Found in sign-in or sign-up responses. This token is used for user sessions |

### Responses

| HTTP status code | content-type    | Description                              |
|------------------|-----------------|------------------------------------------|
| `200`            | application/json| List of messages in the chat.            |
| `500`            | text/plain      | Unknown server error.                    |

### Response object

| Attribute name | Type   | Description                   |
|----------------|--------|-------------------------------|
| `messages`     | array  | List of messages in the chat.  |

#### Message Object Structure

Each message contains:

| Attribute name | Type    | Description                           |
|----------------|---------|---------------------------------------|
| `id`           | integer | Message ID                            |
| `text`         | string  | Message text                          |
| `user_id`      | integer | ID of the user who sent the message   |
| `created_at`   | string  | Timestamp when the message was created |
| `read_by`      | array   | List of user IDs who have read the message |
| `read`         | boolean | Whether the current user has read the message |

### Examples

!!! example
	#### cURL

	```shell
	curl --request GET \
		--header "authentication-token: abc123" \
		"https://gamefuse.co/api/v3/messages/page/1?chat_id=123"
	```

	#### Response

	```json
	{
	  "messages": [
		{
		  "id": 1,
		  "text": "Hello!",
		  "user_id": 10,
		  "created_at": "2024-09-20T12:00:00Z",
		  "read_by": [10, 11],
		  "read": true
		},
		{
		  "id": 2,
		  "text": "Hi there!",
		  "user_id": 11,
		  "created_at": "2024-09-20T12:05:00Z",
		  "read_by": [10],
		  "read": false
		}
	  ]
	}
	```

---

## Send a Message

### Scope

Send a new message to an existing chat.

### Method

!!! info annotate "POST"
	```plaintext
	/api/v3/messages
	```

### Attributes

| Name      | Type    | Required | Description               |
|-----------|---------|----------|---------------------------|
| `text`    | string  | Yes      | The message text to send.  |
| `chat_id` | integer | Yes      | The chat ID to send the message to. |

### Headers

| Name                  | Type   | Description |
|-----------------------|--------|-------------|
| `authentication-token` | string | Found in sign-in or sign-up responses. This token is used for user sessions |
| `Content-Type`         | string | Set it to `application/json` |

### Responses

| HTTP status code | content-type    | Description                              |
|------------------|-----------------|------------------------------------------|
| `200`            | application/json| Message sent successfully.               |
| `422`            | text/plain      | Invalid parameters or missing attributes.|
| `500`            | text/plain      | Unknown server error.                    |

### Response object

| Attribute name | Type   | Description                    |
|----------------|--------|--------------------------------|
| `message`      | object | The newly created message object. |

### Examples

!!! example
	#### cURL

	```shell
	curl --request POST \
		--header "authentication-token: abc123" \
		--header "Content-Type: application/json" \
		--data '{"text": "Hello everyone!", "chat_id": 123}' \
		"https://gamefuse.co/api/v3/messages"
	```

	#### Response

	```json
	{
	  "message": {
		"id": 3,
		"text": "Hello everyone!",
		"user_id": 10,
		"created_at": "2024-09-20T12:34:56Z",
		"read_by": [10],
		"read": true
	  }
	}
	```

---

## Mark a Message as Read

### Scope

Mark a specific message in a chat as read by the current user.

### Method

!!! info annotate "POST"
	```plaintext
	/api/v3/messages/{id}/mark_as_read
	```

### Attributes

| Name | Type    | Required | Description                 |
|------|---------|----------|-----------------------------|
| `id` | integer | Yes      | The ID of the message to mark as read. |

### Headers

| Name                  | Type   | Description |
|-----------------------|--------|-------------|
| `authentication-token` | string | Found in sign-in or sign-up responses. This token is used for user sessions |
| `Content-Type`         | string | Set it to `application/json` |

### Responses

| HTTP status code | content-type    | Description                               |
|------------------|-----------------|-------------------------------------------|
| `200`            | application/json| Message successfully marked as read.      |
| `422`            | text/plain      | Message already marked as read or invalid ID. |
| `500`            | text/plain      | Unknown server error.                     |

### Response object

| Attribute name | Type   | Description                    |
|----------------|--------|--------------------------------|
| `message`      | string | Success message indicating the message has been marked as read. |

### Examples

!!! example
	#### cURL

	```shell
	curl --request POST \
		--header "authentication-token: abc123" \
		"https://gamefuse.co/api/v3/messages/1/mark_as_read"
	```

	#### Response

	```json
	{
	  "message": "Message marked as read"
	}
	```