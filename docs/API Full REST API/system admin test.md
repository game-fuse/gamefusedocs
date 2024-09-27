# System Admin Test Suite API

The Test Suite API allows system admins to perform testing actions such as creating games, users, and cleaning up test data. These functions are only accessible to system admins with the appropriate service key.

---

## Create a Game

### Scope

Create a new game in the system. This will also create an associated account holder and link the game to the service key for easy retrieval in subsequent requests.

### Method

!!! info annotate "POST"
	```plaintext
	/api/v3/test_suite/create_game
	```

### Headers

| Name                  | Type   | Description |
|-----------------------|--------|-------------|
| `authentication-token` | string | Found in sign-in or sign-up responses. This token is used for system admin sessions |
| `Content-Type`         | string | Set it to `application/json` |

### Responses

| HTTP status code | content-type    | Description                      |
|------------------|-----------------|----------------------------------|
| `201`            | application/json| Game created successfully.       |
| `401`            | text/plain      | Unauthorized access.             |

### Response object

| Attribute name | Type    | Description                       |
|----------------|---------|-----------------------------------|
| `id`           | integer | The ID of the created game.       |
| `name`         | string  | The name of the created game.     |
| `token`        | string  | Game authentication token.        |

### Examples

!!! example
	#### cURL

	```shell
	curl --request POST \
		--header "authentication-token: abc123" \
		--header "Content-Type: application/json" \
		"https://gamefuse.co/api/v3/test_suite/create_game"
	```

	#### Response

	```json
	{
	  "id": 1,
	  "name": "Test Game",
	  "token": "abc123"
	}
	```

---

## Create a User

### Scope

Create a new user for a specific game. This is used for testing purposes to quickly add users to a game.

### Method

!!! info annotate "POST"
	```plaintext
	/api/v3/test_suite/create_user
	```

### Attributes

| Name      | Type    | Required | Description                  |
|-----------|---------|----------|------------------------------|
| `game_id` | integer | Yes      | ID of the game to associate the user with. |
| `username`| string  | Yes      | Username of the new user.     |
| `email`   | string  | Yes      | Email of the new user.        |

### Headers

| Name                  | Type   | Description |
|-----------------------|--------|-------------|
| `authentication-token` | string | Found in sign-in or sign-up responses. This token is used for system admin sessions |
| `Content-Type`         | string | Set it to `application/json` |

### Responses

| HTTP status code | content-type    | Description                      |
|------------------|-----------------|----------------------------------|
| `201`            | application/json| User created successfully.       |
| `401`            | text/plain      | Unauthorized access.             |

### Response object

| Attribute name | Type    | Description                       |
|----------------|---------|-----------------------------------|
| `id`           | integer | The ID of the created user.       |
| `username`     | string  | The username of the user.         |
| `email`        | string  | The email of the user.            |

### Examples

!!! example
	#### cURL

	```shell
	curl --request POST \
		--header "authentication-token: abc123" \
		--header "Content-Type: application/json" \
		--data '{"game_id": 1, "username": "test_user", "email": "test_user@example.com"}' \
		"https://gamefuse.co/api/v3/test_suite/create_user"
	```

	#### Response

	```json
	{
	  "id": 10,
	  "username": "test_user",
	  "email": "test_user@example.com"
	}
	```

---

## Clean Up Test Data

### Scope

Delete a game and all associated data (users, game rounds, etc.). This is a testing utility used to quickly clean up the data created during tests.

### Method

!!! info annotate "DELETE"
	```plaintext
	/api/v3/test_suite/clean_up_test
	```

### Attributes

| Name      | Type    | Required | Description                  |
|-----------|---------|----------|------------------------------|
| `game_id` | integer | Yes      | The ID of the game to delete. |

### Headers

| Name                  | Type   | Description |
|-----------------------|--------|-------------|
| `authentication-token` | string | Found in sign-in or sign-up responses. This token is used for system admin sessions |
| `Content-Type`         | string | Set it to `application/json` |

### Responses

| HTTP status code | content-type    | Description                      |
|------------------|-----------------|----------------------------------|
| `200`            | application/json| Game and associated data deleted successfully. |
| `401`            | text/plain      | Unauthorized access.             |

### Response object

| Attribute name | Type   | Description                         |
|----------------|--------|-------------------------------------|
| `message`      | string | Success message confirming that the game and associated data were deleted. |

### Examples

!!! example
	#### cURL

	```shell
	curl --request DELETE \
		--header "authentication-token: abc123" \
		--header "Content-Type: application/json" \
		--data '{"game_id": 1}' \
		"https://gamefuse.co/api/v3/test_suite/clean_up_test"
	```

	#### Response

	```json
	{
	  "message": "Everything should have been destroyed!"
	}
	```

---

This Test Suite API document outlines the key administrative operations for creating games and users, and for cleaning up test data. These operations are restricted to system admins.