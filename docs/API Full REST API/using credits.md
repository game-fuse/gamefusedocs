Credits are a numeric attribute of each game user. It is a simple integer value
that you can add manually.

Credits are automatically detracted upon store item purchases. You can manually
add and remove them via an API call.

## Adding Credits

### Scope

Alter the relative amount of credits a users has.

### Method

!!! info annotate "POST"
    ```plaintext
    /api/v2/users/{signedInUserId}/add_credits?credits={credits}
    ```

### Attributes

| Name             | Type          | Required | Description |
|------------------|---------------|----------|-------------|
| `credits`        | integer       | Yes      | The amount of credits (positive or negative) you want to alter the user's current credits by |
| `signedInUserId` | integer       | Yes      |  The user id value from the GameFuse game dashboard |

### Data (payload)

None

### Headers

| Name | Type | Description |
|----------|---------|--------------|
| `Authentication-Token` | string | Found in sign-in or sign-up responses. This token is used for user sessions |

### Responses

| HTTP status code | content-type | Description |
|------------------|--------------|-------------|
| `200`              | application/json         | Object containing the user's remaining credits and a list of all their purchased store items |
| `400`              | text/plain | `credits` attribute missing |
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
        "https://gamefuse.co/api/v2/users/1/add_credits?credits=10"
    ```

    #### Response

    ```json
    {
        "id": 1,
        "username": "some_username",
        "email": "_appid_1_john.doe@example.com",
        "display_email": "john.doe@example.com",
        "credits": 10,
        "score": 0,
        "last_login": "2024-07-21T14:23:37.457-04:00",
        "number_of_logins": 0,
        "authentication_token": "abc123",
        "events_total": 0,
        "events_current_month": 0,
        "game_sessions_total": 0,
        "game_sessions_current_month": 0
    }
    ```

## Setting Credits

### Scope

Set the absolute amount of credits a users has.

!!! note
    The `credits` parameter you set here will be new total credit amount
    of the user.

### Method

!!! info annotate "POST"
    ```plaintext
    /api/v2/users/{signedInUserId}/set_credits?credits={credits}
    ```

### Attributes

| Name             | Type          | Required | Description |
|------------------|---------------|----------|-------------|
| `credits`        | integer       | Yes      | The amount of credits a user will have |
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
| `200`              | application/json         | Object containing the user's remaining credits and a list of all their purchased store items |
| `400`              | text/plain | `credits` attribute missing |
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
        "https://gamefuse.co/api/v2/users/1/set_credits?credits=10"
    ```

    #### Response

    ```json
    {
        "id": 1,
        "username": "some_username",
        "email": "_appid_1_john.doe@example.com",
        "display_email": "john.doe@example.com",
        "credits": 10,
        "score": 0,
        "last_login": "2024-07-21T14:23:37.457-04:00",
        "number_of_logins": 0,
        "authentication_token": "abc123",
        "events_total": 0,
        "events_current_month": 0,
        "game_sessions_total": 0,
        "game_sessions_current_month": 0
    }
    ```
