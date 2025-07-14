# Game Round API

The Game Round API allows users to manage game rounds in the GameFuse platform. This includes creating game rounds, updating them, viewing a user's game rounds, and deleting game rounds. Game rounds can also be part of multiplayer rounds.

!!! note
    This feature is not supported in the js client library yet.	
## Create a Game Round

### Scope

Create a new game round for a user. Game rounds can either be individual or part of a multiplayer round.  Multiplayer game rounds are all connected to the first one submitted.  Whichever user submits the first multiplayer round that the others connect to is the "master" of the general multiplayer round.  The master user can modify any of the connected multiplayer rounds.  Place is automatically generated for multiplayer rounds based on "score".  When getting game round info, multiplayer rounds will come with an additional payload called "rankings", which is sorting the individual round by score. Updates to already created rounds are supported and rankings will be updated for all when any one object in the connected multiplayer round is updated.

### Method

!!! info annotate "POST"
	/api/v3/game_rounds

### Attributes

| Name                       | Type    | Required | Description                                                                       |
|----------------------------|---------|----------|-----------------------------------------------------------------------------------|
| `game_user_id`              | integer | Yes      | ID of the user to whom the game round belongs.                                    |
| `game_type`                 | string  | Yes      | Type of game being played.                                                        |
| `start_time`                | string  | No       | Start time of the game round.                                                     |
| `end_time`                  | string  | No       | End time of the game round.                                                       |
| `score`                     | integer | No       | The score achieved in the game round.                                             |
| `place`                     | integer | No       | The place the user finished in during the game round (1st, 2nd, etc.).            |
| `multiplayer_game_round_id` | integer | No       | ID of the associated multiplayer game round if applicable.                        |
| `metadata`                  | object  | No       | Additional metadata related to the game round.                                    |
| `multiplayer`               | boolean | No       | If `true`, create or join a multiplayer round.                                    |

### Headers

| Name                  | Type   | Description |
|-----------------------|--------|-------------|
| `authentication-token` | string | Found in sign-in or sign-up responses. This token is used for user sessions |
| `Content-Type`         | string | Set it to `application/json` |

### Responses

| HTTP status code | content-type    | Description                                 |
|------------------|-----------------|---------------------------------------------|
| `201`            | application/json| Game round created successfully.            |
| `422`            | text/plain      | Validation errors or missing parameters.    |
| `500`            | text/plain      | Unknown server error.                       |

### Response object

| Attribute name | Type    | Description                               |
|----------------|---------|-------------------------------------------|
| `id`           | integer | The ID of the created game round.         |
| `game_user_id` | integer | The ID of the user to whom the game round belongs. |
| `start_time`   | string  | The start time of the game round.         |
| `end_time`     | string  | The end time of the game round.           |
| `score`        | integer | The score achieved in the game round.     |
| `place`        | integer | The place finished in the game round.     |
| `game_type`    | string  | The type of game played.                  |
| `metadata`     | object  | Additional metadata related to the game round. |

### Example 1: Creating a Non-Multiplayer Game
!!! example
	
	#### Request
	```shell
	curl --request POST \
		--header "authentication-token: abc123" \
		--header "Content-Type: application/json" \
		--data '{"game_user_id": 1, "start_time": "2024-09-20T10:00:00Z", "end_time": "2024-09-20T10:30:00Z", "score": 1000, "place": 1, "game_type": "solo_adventure", "metadata": {"difficulty": "Medium"}}' \
		"https://gamefuse.co/api/v3/game_rounds"
	```
	#### Response
	```json
	{
  		"id": 101,
  		"game_user_id": 1,
  		"start_time": "2024-09-20T10:00:00Z",
  		"end_time": "2024-09-20T10:30:00Z",
  		"score": 1000,
  		"place": 1,
  		"game_type": "solo_adventure",
  		"metadata": {
			"difficulty": "Medium"
  		}
	}
	```

### Example 2: Creating a Multiplayer Game, then Adding More Game Rounds

!!!example
	#### Step 1: Create the First Multiplayer Game Round
	
	This creates a multiplayer game round if one does not already exist.
	
	#### Request
	```shell
	curl --request POST \
		--header "authentication-token: abc123" \
		--header "Content-Type: application/json" \
		--data '{"game_user_id": 1, "start_time": "2024-09-20T10:00:00Z", "end_time": "2024-09-20T10:30:00Z", "score": 1200, "place": 1, "game_type": "multiplayer_battle", "metadata": {"difficulty": "Hard"}, "multiplayer": true}' \
		"https://gamefuse.co/api/v3/game_rounds"
	```
	#### Response
	```json
	{
  		"id": 102,
  		"game_user_id": 1,
  		"start_time": "2024-09-20T10:00:00Z",
  		"end_time": "2024-09-20T10:30:00Z",
  		"score": 1200,
  		"place": 1,
  		"game_type": "multiplayer_battle",
  		"multiplayer_game_round_id": 201,
  		"metadata": {
			"difficulty": "Hard"
  		}
	}
	```

	#### Step 2: Add Additional Game Rounds to the Multiplayer Game
	
	Now that the multiplayer game round has been created (ID: 201), additional rounds can be added by referencing the `multiplayer_game_round_id`.
	
	#### Request
	```shell
	curl --request POST \
		--header "authentication-token: abc123" \
		--header "Content-Type: application/json" \
		--data '{"game_user_id": 2, "start_time": "2024-09-20T10:30:00Z", "end_time": "2024-09-20T11:00:00Z", "score": 1100, "place": 2, "game_type": "multiplayer_battle", "metadata": {"difficulty": "Hard"}, "multiplayer_game_round_id": 201}' \
		"https://gamefuse.co/api/v3/game_rounds"
	```
	#### Response
	```json
	{
  		"id": 103,
  		"game_user_id": 2,
  		"start_time": "2024-09-20T10:30:00Z",
  		"end_time": "2024-09-20T11:00:00Z",
  		"score": 1100,
  		"place": 2,
  		"game_type": "multiplayer_battle",
  		"multiplayer_game_round_id": 201,
  		"metadata": {
			"difficulty": "Hard"
  		}
	}
	```

## Multiplayer Game Round Response Example

!!!example
	When querying a multiplayer game round, the response includes the rankings of all participants.
	
	### Scope
	
	Retrieve details about a multiplayer game round, including rankings.
	
	### Method
	
	!!! info annotate "GET"
		/api/v3/game_rounds/{id}
	
	#### Example Request
	```shell
	curl --request GET \
		--header "authentication-token: abc123" \
		"https://gamefuse.co/api/v3/game_rounds/102"
	```
	#### Example Response
	```json
	{
  		"id": 102,
  		"game_user_id": 1,
  		"start_time": "2024-09-20T10:00:00Z",
  		"end_time": "2024-09-20T10:30:00Z",
  		"score": 1200,
  		"place": 1,
  		"game_type": "multiplayer_battle",
  		"multiplayer_game_round_id": 201,
  		"metadata": {
			"difficulty": "Hard"
  		},
  		"rankings": [
			{
	  		"place": 1,
	  		"score": 1200,
	  		"start_time": "2024-09-20T10:00:00Z",
	  		"end_time": "2024-09-20T10:30:00Z",
	  		"user": {
				"id": 1,
				"username": "player_one",
				"email": "player_one@example.com",
				"display_email": "player_one@example.com",
				"credits": 100,
				"score": 1500
	  		}
			},
			{
	  		"place": 2,
	  		"score": 1100,
	  		"start_time": "2024-09-20T10:30:00Z",
	  		"end_time": "2024-09-20T11:00:00Z",
	  		"user": {
				"id": 2,
				"username": "player_two",
				"email": "player_two@example.com",
				"display_email": "player_two@example.com",
				"credits": 120,
				"score": 1400
	  		}
			}
  		]
	}
	```

## Show a Game Round

### Scope

Retrieve details about a specific game round.

### Method

!!! info annotate "GET"
	/api/v3/game_rounds/{id}

### Headers

| Name                  | Type   | Description |
|-----------------------|--------|-------------|
| `authentication-token` | string | Found in sign-in or sign-up responses. This token is used for user sessions |

### Responses

| HTTP status code | content-type    | Description                                              |
|------------------|-----------------|----------------------------------------------------------|
| `200`            | application/json| Game round details retrieved successfully.               |
| `401`            | text/plain      | Unauthorized: You cannot view game rounds from another game. |
| `404`            | text/plain      | Game round not found.                                    |
| `500`            | text/plain      | Unknown server error.                                    |

### Response object

| Attribute name         | Type    | Description                                        |
|------------------------|---------|----------------------------------------------------|
| `id`                   | integer | The ID of the game round.                          |
| `game_user_id`         | integer | The ID of the user to whom the game round belongs. |
| `start_time`           | string  | The start time of the game round.                  |
| `end_time`             | string  | The end time of the game round.                    |
| `score`                | integer | The score achieved in the game round.              |
| `place`                | integer | The place finished in the game round.              |
| `game_type`            | string  | The type of game played.                           |
| `multiplayer_game_round_id` | integer | ID of the associated multiplayer game round, if applicable. |
| `metadata`             | object  | Additional metadata related to the game round.     |

### Example

!!! example
	#### cURL
	```shell
	curl --request GET \
		--header "authentication-token: abc123" \
		"https://gamefuse.co/api/v3/game_rounds/101"
	```
	#### Response
	```json
	{
  		"id": 101,
  		"game_user_id": 1,
  		"start_time": "2024-09-20T10:00:00Z",
  		"end_time": "2024-09-20T11:00:00Z",
  		"score": 1500,
  		"place": 1,
  		"game_type": "battle",
  		"metadata": {
			"level": "Hard"
  		},
  		"multiplayer_game_round_id": null
	}
	```

### Multiplayer Example

!!! example
	#### cURL
	```shell
	curl --request GET \
		--header "authentication-token: abc123" \
		"https://gamefuse.co/api/v3/game_rounds/971"
	```
	#### Response
	```json
	{
		"id"=>971,
		"game_id"=>12892,
		"game_user_id"=>30342,
		"multiplayer_game_round_id"=>275,
		"game_type"=>"Fingerlime",
		"place"=>1,
		"score"=>100.0,
		"start_time"=>"2025-05-15T11:43:28.599-04:00",
		"end_time"=>"2025-05-15T11:49:28.599-04:00",
		"metadata"=>nil,
		"created_at"=>"2025-05-15T11:53:28.602-04:00",
		"updated_at"=>"2025-05-15T11:53:28.602-04:00",
		"rankings"=>[
			{
			"id"=>971,
			"place"=>1,
			"score"=>100.0,
			"start_time"=>"2025-05-15T11:43:28.599-04:00",
			"end_time"=>"2025-05-15T11:49:28.599-04:00",
			"user"=>{
				"id"=>30342,
				"username"=>"Judy Kovacek",
				"credits"=>0,
				"score"=>0,
				"email"=>"truman@schneider.io",
				"is_new_user"=>true
				}
			},
			{
			"id"=>972,
			"place"=>2,
			"score"=>200.0,
			"start_time"=>"2025-05-15T11:43:28.622-04:00",
			"end_time"=>"2025-05-15T11:49:28.622-04:00",
			"user"=>{
				"id"=>30343,
				"username"=>"Arnetta Schmeler",
				"credits"=>0,
				"score"=>0,
				"email"=>"nicola@dare.net",
				"is_new_user"=>true
				}
			},
			{
			"id"=>973,
			"place"=>3,
			"score"=>300.0,
			"start_time"=>"2025-05-15T11:43:28.647-04:00",
			"end_time"=>"2025-05-15T11:49:28.647-04:00",
			"user"=>{
				"id"=>30344,
				"username"=>"Lorene Dickinson",
				"credits"=>0,
				"score"=>0,
				"email"=>"allan.oconner@keebler.biz",
				"is_new_user"=>true
				}
			}
		]
	}
	```

## Update a Game Round

### Scope

Update an existing game round for the current user.

### Method

!!! info annotate "PUT"
	/api/v3/game_rounds/{id}

### Attributes

| Name                       | Type    | Required | Description                                                                       |
|----------------------------|---------|----------|-----------------------------------------------------------------------------------|
| `id`                        | integer | Yes      | ID of the game round to update.                                                   |
| `start_time`                | string  | No       | Start time of the game round.                                                     |
| `end_time`                  | string  | No       | End time of the game round.                                                       |
| `score`                     | integer | No       | The score achieved in the game round.                                             |
| `place`                     | integer | No       | The place the user finished in during the game round.                             |
| `game_type`                 | string  | No       | Type of game being played.                                                        |
| `metadata`                  | object  | No       | Additional metadata related to the game round.                                    |

### Headers

| Name                  | Type   | Description |
|-----------------------|--------|-------------|
| `authentication-token` | string | Found in sign-in or sign-up responses. This token is used for user sessions |
| `Content-Type`         | string | Set it to `application/json` |

### Responses

| HTTP status code | content-type    | Description                              |
|------------------|-----------------|------------------------------------------|
| `200`            | application/json| Game round updated successfully.         |
| `401`            | text/plain      | Unauthorized: Only the owner can update the game round. |
| `422`            | text/plain      | Validation errors or missing parameters. |
| `500`            | text/plain      | Unknown server error.                    |

### Response object

The response will return the updated game round data, similar to the create response.

### Examples

!!! example
	#### cURL
	```shell
	curl --request PUT \
		--header "authentication-token: abc123" \
		--header "Content-Type: application/json" \
		--data '{"score": 1600, "place": 2}' \
		"https://gamefuse.co/api/v3/game_rounds/101"
	```
	#### Response
	```json
	{
  		"id": 101,
  		"game_user_id": 1,
  		"start_time": "2024-09-20T10:00:00Z",
  		"end_time": "2024-09-20T11:00:00Z",
  		"score": 1600,
  		"place": 2,
  		"game_type": "battle",
  		"metadata": {
			"level": "Hard"
  		}
	}
	```

## View a User's Game Rounds

### Scope

Retrieve a list of game rounds for a specific user. Multiplayer game rounds retrieved in bulk will not have rankings attached.  To retrieve a game rounds rankings, a seperate call must be made to the #show route

### Method

!!! info annotate "GET"
	/api/v3/game_rounds?user_id={user_id}

!!! tip annotate "NOTE"
    ```plaintext
    Other users in the same game can be queried with this route.
    ```


### Attributes

| Name     | Type    | Required | Description                         |
|----------|---------|----------|-------------------------------------|
| `user_id`| integer | Yes      | ID of the user whose game rounds to retrieve. |
| `page`   | integer | No       | Page number (default 1) |
| `per_page`| integer | No       | Number of game rounds returned in a single query (default and max 100) |

### Headers

| Name                  | Type   | Description |
|-----------------------|--------|-------------|
| `authentication-token` | string | Found in sign-in or sign-up responses. This token is used for user sessions |

### Responses

| HTTP status code | content-type    | Description                              |
|------------------|-----------------|------------------------------------------|
| `200`            | application/json| List of game rounds for the user.        |
| `401`            | text/plain      | Unauthorized: You cannot view another game's data. |
| `422`            | text/plain      | Missing user ID parameter.               |
| `500`            | text/plain      | Unknown server error.                    |

### Response object

| Attribute name | Type    | Description                        |
|----------------|---------|------------------------------------|
| `id`           | integer | The ID of the game round.          |
| `game_user_id` | integer | The ID of the user to whom the game round belongs. |
| `start_time`   | string  | The start time of the game round.  |
| `end_time`     | string  | The end time of the game round.    |
| `score`        | integer | The score achieved in the game round. |
| `place`        | integer | The place finished in the game round. |
| `game_type`    | string  | The type of game played.           |
| `metadata`     | object  | Additional metadata related to the game round. |

### Examples

!!! example
	#### cURL
	```shell
	curl --request GET \
		--header "authentication-token: abc123" \
		"https://gamefuse.co/api/v3/game_rounds?user_id=1"
	```
	#### Response
	```json
	{
		"game_rounds": [
			{
				"id": 101,
				"game_user_id": 1,
				"start_time": "2024-09-20T10:00:00Z",
				"end_time": "2024-09-20T11:00:00Z",
				"score": 1500,
				"place": 1,
				"game_type": "battle",
				"metadata": {
				"level": "Hard"
				}
			},
			{
				"id": 102,
				"game_user_id": 1,
				"start_time": "2024-09-21T10:00:00Z",
				"end_time": "2024-09-21T11:00:00Z",
				"score": 1700,
				"place": 1,
				"game_type": "adventure",
				"metadata": {
				"level": "Medium"
				}
			}
		]
	}
	```

## Delete a Game Round

### Scope

Delete an existing game round for the current user.

### Method

!!! info annotate "DELETE"
	/api/v3/game_rounds/{id}

### Attributes

| Name | Type    | Required | Description                  |
|------|---------|----------|------------------------------|
| `id` | integer | Yes      | The ID of the game round to delete. |

### Headers

| Name                  | Type   | Description |
|-----------------------|--------|-------------|
| `authentication-token` | string | Found in sign-in or sign-up responses. This token is used for user sessions |

### Responses

| HTTP status code | content-type    | Description                                      |
|------------------|-----------------|--------------------------------------------------|
| `200`            | application/json| Game round deleted successfully.                 |
| `401`            | text/plain      | Unauthorized: Only the owner can delete the game round. |
| `500`            | text/plain      | Unknown server error.                            |

### Response object

| Attribute name | Type   | Description                    |
|----------------|--------|--------------------------------|
| `message`      | string | Success message confirming the game round was deleted. |

### Examples

!!! example
	#### cURL
	```shell
	curl --request DELETE \
		--header "authentication-token: abc123" \
		"https://gamefuse.co/api/v3/game_rounds/101"
	```
	#### Response
	```json
	{
  		"message": "Game Round destroyed successfully!"
	}
	```
