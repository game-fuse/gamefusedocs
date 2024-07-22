After sign up, you should provide a sign in function for users to log-in at
a later time.

## Sign in users

### Scope

Make users log-in.

!!! important
    This step returns the `Authentication-Token` HTTP header which is to be
    used for most API requests.

### Method

!!! info annotate "POST"
    ```plaintext
    /api/v2/sessions?email={email}&password={password}&game_id={gameId}&game_token={gameToken}
    ```

### Attributes

| Name             | Type          | Required | Description |
|------------------|---------------|----------|-------------|
| `email`          | string        | Yes      | User's email, used for login and forgot password functionality |
| `gameId`         | integer       | Yes      | Value found on your GameFuse.co dashboard |
| `gameToken`      | string        | Yes      | API token found on your GameFuse.co dashboard |
| `password`       | string        | Yes      | User's password |

### Headers

None

### Responses

| HTTP status code | content-type | Description |
|------------------|--------------|-------------|
| `200`              | application/json         | Object containing the user's remaining scores and a list of all their purchased store items |
| `402`              | text/plain | Game is disabled. Check the GameFuse dashboard |
| `404`              | text/plain | User not found or incorrect password |
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
| `id`                              | integer | User's id   |
| `last_login`                      | string | Timestamp of last login |
| `number_of_logins`                | integer | Total logins |
| `score`                           | integer | A generic score metric |
| `username`                        | string  | User's display username |

### Examples

!!! example
    ### cURL

    ```shell
    curl --request POST \
        "https://gamefuse.co/api/v2/sessions?email=john.doe@example.com&password=1234abcd&game_id=1&game_token=abc123
    ```

    ### Response

    ```json
    {
        "id": 1,
        "username": "john.doe",
        "email": "_appid_1_john.doe@example.com",
        "display_email": "john.doe@example.com",
        "credits": 0,
        "score": 0,
        "last_login": "2024-07-21",
        "number_of_logins": 1,
        "authentication_token": "abc123",
        "events_total": 1,
        "events_current_month": 1,
        "game_sessions_total": 1,
        "game_sessions_current_month": 1
    }
    ```
