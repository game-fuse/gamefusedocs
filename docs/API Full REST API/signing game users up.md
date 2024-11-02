Once your account is verified, you should take note of the API token and ID.
You will need them on each subsequent request.

## Sign up users

### Scope

Sign up a new user in your game.

!!! important
    This step returns the `authentication-token` HTTP header which is to be
    used for most API requests.

### Method

!!! info annotate "POST"
    ```plaintext
    /api/v3/users
    ```

### Attributes

None

### Data (payload)

| Name             | Type          | Required | Description |
|------------------|---------------|----------|-------------|
| `email`          | string        | Yes      | User's email, used for login and forgot password functionality |
| `gameId` | Value found on your GameFuse.co dashboard |
| `gameToken`      | string        | Yes      | API token found on your GameFuse.co dashboard |
| `password`       | string        | Yes      | User's password |
| `passwordConfirmation`   | string        | Yes      | Same as `password` |
| `username` | string | Yes | User's display name used on leaderboards. It must be unique for your game |

### Headers

| Name | Type | Description |
|----------|---------|--------------|
| `Content-Type`         | string | Set it to `application/json` |

### Responses

| HTTP status code | content-type | Description |
|------------------|--------------|-------------|
| `200`              | application/json         | Object containing the user's remaining scores and a list of all their purchased store items |
| `404`              | text/plain | Failed to fetch game variables. `gameId` or `gameToken` might be wrong |
| `500`              | text/plain | Unknown server error |

### Response object

| Attribute name                    | Type | Description |
|-----------------------------------|------|-------------|
| `authentication_token`            | string | Token that must be saved and added as a parameter to all authenticated requests |
| `credits`                         | integer | Number of credits the user has. These can be used in your in game store |
| `email`                           | string  | Users email field added on registration |
| `id`                              | integer | User's id   |
| `last_login`                      | string | Timestamp of last login |
| `number_of_logins`                | integer | Total logins |
| `score`                           | integer | A generic score metric |
| `username`                        | string  | User's display username |

### Example

!!! example
    #### cURL

    ```shell
    curl --request POST \
        --header 'Content-Type: application/json' \
        --data '{"email: "john.doe@example.com", "password": 1234", "password_confirmation": "1234", "username": "jdoe", "game_id": 1, "game_token": "abc123"}' \
        https://gamefuse.co/api/v2/users
    ```

    #### Response

    ```json
    {
        "id": 1,
        "username": "some_username",
        "email": "_appid_1_john.doe@example.com",
        "display_email": "john.doe@example.com",
        "credits": 0,
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
