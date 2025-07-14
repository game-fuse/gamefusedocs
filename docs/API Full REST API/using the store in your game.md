## Getting Available Store Items

### Scope

Get all the store items from a specific game.

### Method

!!! info annotate "GET"
    ```plaintext
    /api/v3/games/store_items?game_id={gameId}&game_token={gameToken}
    ```

### Attributes

| Name             | Type          | Required | Description |
|------------------|---------------|----------|-------------|
| `gameId` | Value found on your GameFuse.co dashboard |
| `gameToken`      | string        | Yes      | API token found on your GameFuse.co dashboard |

### Data (payload)

None

### Headers

None

### Responses

| HTTP status code | content-type | Description |
|------------------|--------------|-------------|
| `200`              | application/json         | Object containing the game store items |
| `404`              | text/plain | Failed to fetch game variables. `gameId` or `gameToken` might be wrong |
| `500`              | text/plain | Unknown server error |

### Response object

| Attribute name                    | Type | Description |
|-----------------------------------|------|-------------|
| `store_items`            | list | A list of objects where each one represents the fields of a store item |

### Example

!!! example
    #### cURL

    ```shell
    curl --request GET \
        "https://gamefuse.co/api/v3/games/store_items?game_id=1&game_token=abc123"
    ```

    #### Response

    ```json
    {
      "store_items": [
        {
          "id": 337,
          "name": "test",
          "cost": 123,
          "description": "a test item",
          "category": "generic",
          "icon_url": null
        },
        {
          "id": 338,
          "name": "test 2",
          "cost": 321,
          "description": "another test item",
          "category": "main",
          "icon_url": null
        }
      ]
    }
    ```

## Purchasing a Store Item

### Scope

Purchase a store item.

!!! important
    A user must be signed in and have enough credits to call this endpoint! 

### Method

!!! info annotate "POST"
    ```plaintext
    /api/v3/users/{signedInUserId}/purchase_game_user_store_item
    ```

### Attributes

| Name             | Type          | Required | Description |
|------------------|---------------|----------|-------------|
| `signedInUserId` | integer       | Yes      | The user id value from the GameFuse game dashboard |

### Data (payload)

| Name             | Type          | Required | Description |
|------------------|---------------|----------|-------------|
| `store_item_id` | integer       | Yes      | Id of the store item |

### Headers

| Name | Type | Description |
|----------|---------|--------------|
| `authentication-token` | string | Found in sign-in or sign-up responses. This token is used for user sessions |

### Responses

| HTTP status code | content-type | Description |
|------------------|--------------|-------------|
| `200`              | application/json         | Object containing the game store items belonging to the user and the credits |
| `403`              | text/plain | Not enough credits or item already purchased |
| `404`              | text/plain | Item not found |
| `500`              | text/plain | Unknown server error |

### Response object

| Attribute name                    | Type | Description |
|-----------------------------------|------|-------------|
| `credits`                | integer | The user's remaining credits |
| `game_user_store_items`            | list | A list of objects where each one represents the fields of a store item purchased by the user |

### Example

!!! example
    #### cURL

    ```shell
    curl --request POST \
        --header "Content-Type: application/json" \
        --header "authentication-token: abc123" \
        --data '{"store_item_id": 337}' \
        'https://gamefuse.co/api/v3/users/1/purchase_game_user_store_item'
    ```

    #### Response

    ```json
    {
      "credits": 135,
      "store_items": [
        {
          "id": 337,
          "name": "test",
          "cost": 123,
          "description": "a test item",
          "category": "generic",
          "icon_url": null
        },
        {
          "id": 338,
          "name": "test 2",
          "cost": 321,
          "description": "another test item",
          "category": "main",
          "icon_url": null
        }
      ]
    }
    ```

## Removing a Store Item

### Scope

Revoke a store item purchase.

!!! important
    A user must be signed in and have enough credits to call this endpoint! 

### Method

!!! info annotate "GET"
    ```plaintext
    /api/v3/users/{signedInUserId}/remove_game_user_store_item?store_item_id={storeItemId}
    ```

### Attributes

| Name             | Type          | Required | Description |
|------------------|---------------|----------|-------------|
| `signedInUserId` | integer       | Yes      | The user id value from the GameFuse game dashboard |
| `storeItemId` | integer       | Yes      | Id of the store item to remove |

### Data (payload)

None

### Headers

| Name | Type | Description |
|----------|---------|--------------|
| `authentication-token` | string | Found in sign-in or sign-up responses. This token is used for user sessions |

### Responses

| HTTP status code | content-type | Description |
|------------------|--------------|-------------|
| `200`              | application/json         | Object containing the game store items belonging to the user and the credits |
| `404`              | text/plain | Item not found or not previously purchased |
| `500`              | text/plain | Unknown server error |

### Response object

| Attribute name                    | Type | Description |
|-----------------------------------|------|-------------|
| `credits`                | integer | The user's remaining credits |
| `game_user_store_items`            | list | A list of objects where each one represents the fields of a store item purchased by the user |

### Example

!!! example
    #### cURL

    ```shell
    curl --request GET \
        --header "authentication-token: abc123" \
        "https://gamefuse.co/api/v3/users/1/purchase_game_user_store_item?store_item_id=337"
    ```

    #### Response

    ```json
    {
      "credits": 135,
      "store_items": [
        {
          "id": 337,
          "name": "test",
          "cost": 123,
          "description": "a test item",
          "category": "generic",
          "icon_url": null
        },
        {
          "id": 338,
          "name": "test 2",
          "cost": 321,
          "description": "another test item",
          "category": "main",
          "icon_url": null
        }
      ]
    }
    ```

## Getting Purchased Store Items

### Scope

Get all of the user's purchased store items.

!!! important
    A user must be signed in and have enough credits to call this endpoint! 

### Method

!!! info annotate "GET"
    ```plaintext
    /api/v3/users/{signedInUserId}/game_user_store_items
    ```
!!! tip annotate "NOTE"
    ```plaintext
    Other users can be queried with this route
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
| `authentication-token` | string | Found in sign-in or sign-up responses. This token is used for user sessions |

### Responses

| HTTP status code | content-type | Description |
|------------------|--------------|-------------|
| `200`              | application/json         | Object containing the game store items belonging to the user and the credits |
| `500`              | text/plain | Unknown server error |

### Response object

| Attribute name                    | Type | Description |
|-----------------------------------|------|-------------|
| `credits`                | integer | The user's remaining credits |
| `game_user_store_items`            | list | A list of objects where each one represents the fields of a store item purchased by the user |

### Example

!!! example
    #### cURL

    ```shell
    curl --request GET \
        --header "authentication-token: abc123" \
        "https://gamefuse.co/api/v3/users/1/game_user_store_items"
    ```

    #### Response

    ```json
    {
      "credits": 135,
      "store_items": [
        {
          "id": 337,
          "name": "test",
          "cost": 123,
          "description": "a test item",
          "category": "generic",
          "icon_url": null
        },
        {
          "id": 338,
          "name": "test 2",
          "cost": 321,
          "description": "another test item",
          "category": "main",
          "icon_url": null
        }
      ]
    }
    ```
