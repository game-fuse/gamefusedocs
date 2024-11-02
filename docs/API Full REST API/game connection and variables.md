Once an account is created, you can verify your game before using other GameFuse
enpoints.

## GameFuse verification

### Scope

Verify your game at GameFuse.

!!! note
    This is not a required step but it can be useful to verify a connection
    before logging in specific users.

### Method

!!! info annotate "GET"
    ```plaintext
    /api/v3/games/verify?game_id={gameId}&game_token={gameToken}
    ```

### Attributes

| Name             | Type          | Required | Description |
|------------------|---------------|----------|-------------|
| `gameId`         | integer       | Yes      | Value found on your GameFuse.co dashboard |
| `gameToken`      | string        | Yes      | API token found on your GameFuse.co dashboard |

### Headers

| Name | Type | Description |
|----------|---------|--------------|
| `authentication-token` | string | Found in sign-in or sign-up responses. This token is used for user sessions |

### Data (payload)

None

### Responses

| HTTP status code | content-type | Description |
|------------------|--------------|-------------|
| `200`              | application/json         | Object containing the user's game information |
| `401`              | text/plain | Failed to verify game |
| `500`              | text/plain | Unknown server error |

### Response object

| Attribute name                    | Type | Description |
|-----------------------------------|------|-------------|
| `description`               | string  | Game description |
| `game_variables`            | list    | All the games' attributes |
| `id`                        | integer | Unique database ID of the game |
| `name`                      | string  | Name of the game |
| `token`                     | string  | API token of the game |

### Examples

!!! example
    #### cURL

    ```shell
    curl --request GET \
        --header "authentication-token: abc123" \
        'https://gamefuse.co/api/v3/games/verify?game_id=1&game_token=cde456'
    ```

    #### Response

    ```json
    {
        "id": 1,
        "name": "my game",
        "token": "cde456",
        "description": "This is a new game",
        "game_variables": [
        {
            "id": 0,
            "key": "this_key",
            "value": "this_value"
        },
        {
            "id": 1,
            "key": "other_key",
            "value": "other_value"
        },
        ...
        ]
    }
    ```

## Game Variables

### Scope

Retrieve game data.

!!! important
    The returned data is the same as the one in the verification endpoint.

### Method

!!! info annotate "GET"
    ```plaintext
    /api/v3/games/fetch_game_variables?game_id={gameId}&game_token={gameToken}
    ```

### Attributes

| Name             | Type          | Required | Description |
|------------------|---------------|----------|-------------|
| `gameId`         | integer       | Yes      | Value found on your GameFuse.co dashboard |
| `gameToken`      | string        | Yes      | API token found on your GameFuse.co dashboard |

### Data (payload)

None

### Headers

| Name | Type | Description |
|----------|---------|--------------|
| `authentication-token` | string | Found in sign-in or sign-up responses. This token is used for user sessions |

### Responses

| HTTP status code | content-type | Description |
|------------------|--------------|-------------|
| `200`              | application/json         | Object containing the user's game information |
| `401`              | text/plain | Failed to fetch game variables. Check your `gameToken` and `gameId` |
| `500`              | text/plain | Unknown server error |

### Response object

| Attribute name                    | Type | Description |
|-----------------------------------|------|-------------|
| `description`               | string  | Game description |
| `game_variables`            | list    | All the games' attributes |
| `id`                        | integer | Unique database ID of the game |
| `name`                      | string  | Name of the game |
| `token`                     | string  | API token of the game |

### Examples

!!! example
    #### cURL

    ```shell
    curl --request GET \
        --header "authentication-token: abc123" \
        'https://gamefuse.co/api/v3/games/fetch_game_variables?game_id=1&game_token=cde456'
    ```

    #### Response

    ```json
    {
        "id": 1,
        "name": "my game",
        "token": "abc123",
        "description": "This is a new game",
        "game_variables": [
        {
            "id": 0,
            "key": "this_key",
            "value": "this_value"
        },
        {
            "id": 1,
            "key": "other_key",
            "value": "other_value"
        },
        ...
        ]
    }
    ```
