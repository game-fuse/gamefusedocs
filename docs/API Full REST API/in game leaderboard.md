## Submit New Leaderboard Entry

### Scope

Add a leaderboard entry to a specific leaderboard within your game.
designated by `leaderboard_name`, these can be pulled later and compared to other users.

!!! note
    Leaderboards can be easily created within GameFuse From your JS game client.

### Method

!!! info annotate "POST"
    ```plaintext
    /api/v2/users/{signedInUserId}/add_leaderboard_entry?score={score}&leaderboard_name={leaderboardName}&extra_attributes={checksum}
    ```

!!! important
    The `extra_attributes` parameters is not required

### Attributes

| Name                     | Type          | Required | Description |
|--------------------------|---------------|----------|-------------|
| `leaderboardName`        | string       | Yes      | Leaderboard name within the game |
| `score`                  | integer      | Yes      | Score for the leaderboard |
| `signedInUserId`         | integer       | Yes      | The user id value from the GameFuse game dashboard |
| `checksum`               | string        | No      | Checksum of custom data (extra data) to save to the leaderboard entry |

### Data (payload)

None

### Headers

| Name | Type | Description |
|----------|---------|--------------|
| `Authentication-Token` | string | Found in sign-in or sign-up responses. This token is used for user sessions |

### Responses

| HTTP status code | content-type | Description |
|------------------|--------------|-------------|
| `200`              | application/json         | Object containing all the user's attributes |
| `400`              | text/plain | Invalid extra attributes |
| `401`              | text/plain | Can only add entries for the current user |
| `500`              | text/plain | Unknown server error |

### Response object

| Attribute name                    | Type | Description |
|-----------------------------------|------|-------------|
| `authentication_token`            | string | Token that must be saved and added as a parameter to all authenticated requests |
| `credits`                         | integer | Number of credits the user has. These can be used in your in game store |
| `display_email`                   | string  | User's actual email used for notifications and login |
| `email`                           | string  | System email: a combination of `id` and `email` |
| `events_total`                    | integer | Running API hits for this user |
| `events_current_month`            | integer | Running API hits for this user for the current month |
| `game_sessions_current_month`     | integer | unique game session for this user during the current month |
| `game_sessions_total`             | integer | Unique game session for this user |
| `id`                              | integer | User's id |
| `last_login`                      | string | Timestamp of last login |
| `number_of_logins`                | integer | Total logins |
| `score`                           | integer | A generic score metric |
| `username`                        | string  | User's display username |

### Examples

!!! example
    #### cURL

    ```shell
    curl --request POST \
        --header "Authentication-Token: abc123" \
        "https://gamefuse.co/api/v2/users/1/add_leaderboard_entry?score=21&leaderboard_name=leaderboard%20test"
    ```

    #### Response

    ```json
    {
        "id": 1,
        "username": "some_username",
        "email": "john.doe-1@example.com",
        "display_email": "john.doe@example.com",
        "credits": 125,
        "score": 10134,
        "last_login": "2022-01-15T10:30:00Z",
        "number_of_logins": 34,
        "authentication_token": "abc123",
        "events_total": 15,
        "events_current_month": 7,
        "game_sessions_total": 51,
        "game_sessions_current_month": 9
    }
    ```

## Clear Leaderboard Entries

### Scope

Clear all leaderboard entries for a specific user.

### Method

!!! info annotate "POST"
    ```plaintext
    /api/v2/users/{signedInUserId}/clear_my_leaderboard_entries
    ```

### Attributes

| Name             | Type          | Required | Description |
|------------------|---------------|----------|-------------|
| `signedInUserId` | integer       | Yes      | The user id value from the GameFuse game dashboard |

### Data (payload)

None

### Headers

| Name | Type | Description |
|----------|---------|--------------|
| `Authentication-Token` | string | Found in sign-in or sign-up responses. This token is used for user sessions |

### Responses

| HTTP status code | content-type | Description |
|------------------|--------------|-------------|
| `200`              | application/json         | Object containing all the user's attributes |
| `401`              | text/plain | Can only clear entries for current user |
| `500`              | text/plain | Unknown server error |

### Response object

| Attribute name                    | Type | Description |
|-----------------------------------|------|-------------|
| `authentication_token`            | string | Token that must be saved and added as a parameter to all authenticated requests |
| `credits`                         | integer | Number of credits the user has. These can be used in your in game store |
| `display_email`                   | string  | User's actual email used for notifications and login |
| `email`                           | string  | System email: a combination of `id` and `email` |
| `events_total`                    | integer | Running API hits for this user |
| `events_current_month`            | integer | Running API hits for this user for the current month |
| `game_sessions_current_month`     | integer | unique game session for this user during the current month |
| `game_sessions_total`             | integer | Unique game session for this user |
| `id`                              | integer | User's id |
| `last_login`                      | string | Timestamp of last login |
| `number_of_logins`                | integer | Total logins |
| `score`                           | integer | A generic score metric |
| `username`                        | string  | User's display username |

### Examples

!!! example
    #### cURL

    ```shell
    curl --request POST \
        --header "Authentication-Token: abc123" \
        "https://gamefuse.co/api/v2/users/1/clear_my_leaderboard_entries"
    ```

    #### Response

    ```json
    {
        "id": 1,
        "username": "some_username",
        "email": "john.doe-1@example.com",
        "display_email": "john.doe@example.com",
        "credits": 125,
        "score": 10134,
        "last_login": "2022-01-15T10:30:00Z",
        "number_of_logins": 34,
        "authentication_token": "abc123",
        "events_total": 15,
        "events_current_month": 7,
        "game_sessions_total": 51,
        "game_sessions_current_month": 9
    }
    ```

## Leaderboard Entries

### Scope

Get leaderboard entries for a specific leaderboard name.

### Method

!!! info annotate "GET"
    ```plaintext
    /api/v2/games/{gameId}/leaderboard_entries?leaderboard_name={leaderboardName}
    ```

!!! important
    You may have multiple leaderboards in your game indicated by `leaderboardName`.

!!! note
    Leaderboards can be downloaded for a specific `leaderboardName`, which
    would gather all the high scores for all users in the game.

### Attributes

| Name             | Type          | Required | Description |
|------------------|---------------|----------|-------------|
| `gameId`         | integer       | Yes      | Found on your GameFuse.co dashboard |
| `leaderboardName` | string       | Yes     | Name of the leaderboard within the game |

### Data (payload)

None

### Headers

| Name | Type | Description |
|----------|---------|--------------|
| `Authentication-Token` | string | Found in sign-in or sign-up responses. This token is used for user sessions |

### Responses

| HTTP status code | content-type | Description |
|------------------|--------------|-------------|
| `200`              | application/json         | Object containing leaderboard entries |
| `400`              | text/plain | No entries for this leaderboard name |
| `500`              | text/plain | Unknown server error |

### Response object

| Attribute name                    | Type | Description |
|-----------------------------------|------|-------------|
| `leaderboard_entries`             | list | Contains leaderboard values and extra attributes |

### Examples

!!! example
    #### cURL

    ```shell
    curl --request GET \
        --header "Authentication-Token: abc123" \
        "https://gamefuse.co/api/v2/games/1/leaderboard_entries?leaderboard_name=some_leaderboard_name"
    ```

    #### Response

    ```json
    {
      "leaderboard_entries": [
          {
              "username": "john.doe",
              "score": 1453,
              "leaderboard_name": "leaderboard_for_game_1",
              "game_user_id": 1,
              "extra_attributes": "bc125ad76"
          },
          {
              "username": "jane.doe",
              "score": 1234,
              "leaderboard_name": "leaderboard_for_game_1",
              "game_user_id": 1,
              "extra_attributes": "de689fa29"
          },
          ...
      ]
    }
    ```

## User-specific leaderboard entries

### Scope

Get all leaderboard entries for a specific user.

### Method

!!! info annotate "GET"
    ```plaintext
    /api/v2/users/{signedInUserId}/leaderboard_entries
    ```

### Attributes

| Name             | Type          | Required | Description |
|------------------|---------------|----------|-------------|
| `signedInUserId` | integer       | Yes      | The user id value from the GameFuse game dashboard |

### Data (payload)

None

### Headers

| Name | Type | Description |
|----------|---------|--------------|
| `Authentication-Token` | string | Found in sign-in or sign-up responses. This token is used for user sessions |

### Responses

| HTTP status code | content-type | Description |
|------------------|--------------|-------------|
| `200`              | application/json         | Object containing the user's remaining scores and a list of all their purchased store items |
| `401`              | text/plain | Can only get entries for the current user |
| `500`              | text/plain | Unknown server error |

### Examples

!!! example
    #### cURL

    ```shell
    curl --request GET \
        --header "Authentication-Token: abc123" \
        "https://gamefuse.co/api/v2/users/1/leaderboard_entries"
    ```

    #### Response

    ```json
    {
      "leaderboard_entries": [
          {
              "username": "john.doe",
              "score": 1453,
              "leaderboard_name": "leaderboard_for_game_1",
              "game_user_id": 1,
              "extra_attributes": "bc125ad76"
          },
          {
              "username": "john.doe",
              "score": 1234,
              "leaderboard_name": "leaderboard_for_game_2",
              "game_user_id": 2,
              "extra_attributes": "0f987aa05"
          },
          ...
      ]
    }
    ```
