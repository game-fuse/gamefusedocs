User data can be set in a number of ways.

Score can be set with a specific API call to set or relatively add to score.
You can also set custom user data where you can assign any key to any value.
Whether you use it for a players `current_level`, `color`, `XP` or anything
else you can think of, it can be done with the custom data.

## Adding scores

### Scope

Alters the amount of scores a users has relatively

### Method

!!! info annotate "POST"
    ```plaintext
    /api/v2/users/{signedInUserId}/add_score
    ```

### Attributes

| Name             | Type          | Required | Description |
|------------------|---------------|----------|-------------|
| `signedInUserId` | integer       | Yes      | The user id value from the GameFuse game dashboard |

### Data (payload)

| Name | Type          | Required | Description |
|------------------|---------------|----------|-------------|
| `score`         | integer       | Yes      | The amount of scores positive or negative you want to alter the users current scores by |

### Headers

| Name | Type | Description |
|----------|---------|--------------|
| `authentication-token` | string | Found in sign-in or sign-up responses. This token is used for user sessions |
| `Content-Type`         | string | Set it to `application/json` |

### Responses

| HTTP status code | content-type | Description |
|------------------|--------------|-------------|
| `200`              | application/json         | Object containing the user's remaining scores and a list of all their purchased store items |
| `400`              | text/plain | `scores` attribute missing |
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
        --header "authentication-token: abc123" \
        --header "Content-Type: application/json" \
        --data '{"score": 10}' \
        "https://gamefuse.co/api/v2/users/1/add_score"
    ```

    #### Response

    ```json
    {
        "id": 1,
        "username": "some_username",
        "email": "_appid_1_john.doe@example.com",
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

## Setting scores

### Scope

Set the absolute amount of scores a user has. The scores param will be the user's new scores total.

### Method

!!! info annotate "POST"                                                        
    ```plaintext
    /api/v2/users/{signedInUserId}/set_score
    ```

### Attributes

| Name             | Type          | Required | Description |
|------------------|---------------|----------|-------------|
| `signedInUserId` | integer       | Yes      | The user id value from the GameFuse game dashboard |

### Data (payload)

| Name | Type          | Required | Description |
|------------------|---------------|----------|-------------|
| `score`         | integer       | Yes      | The amount of scores the user will now have |

### Headers

| Name | Type | Description |
|----------|---------|--------------|
| `authentication-token` | string | Found in sign-in or sign-up responses. This token is used for user sessions |

### Responses

| HTTP status code | content-type | Description |
|------------------|--------------|-------------|
| `200`              | application/json         | Object containing the user's remaining scores and a list of all their purchased store items |
| `400`              | text/plain | `scores` attribute missing |
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

#### cURL

!!! example
    ```shell
    curl --request POST \
        --header "authentication-token: abc123" \
        --header "Content-Type: application/json" \
        --data '{"score": 10}' \
        "https://gamefuse.co/api/v2/users/1/set_score"
    ```

    #### Response

    ```json
    {
        "id": 1,
        "username": "some_username",
        "email": "_appid_1_john.doe@example.com",
        "display_email": "john.doe@example.com",
        "credits": 125,
        "score": 10,
        "last_login": "2022-01-15T10:30:00Z",
        "number_of_logins": 34,
        "authentication_token": "abc123",
        "events_total": 15,
        "events_current_month": 7,
        "game_sessions_total": 51,
        "game_sessions_current_month": 9
    }
    ```

## Adding a custom attribute 

### Scope

Set arbitrary custom attributes. The values of these attributes are in a string
format but can be converted into any type by the programming language in use.

### Method

!!! info annotate "POST"
    ```plaintext
    /api/v2/users/{signedInUserId}/add_game_user_attribute
    ```

### Attributes

| Name             | Type          | Required | Description |
|------------------|---------------|----------|-------------|
| `signedInUserId` | integer       | Yes      | The user id value from the GameFuse game dashboard |

### Data (payload)

| Name             | Type          | Required | Description |
|------------------|---------------|----------|-------------|
| `key`            | string        | Yes      | The key of the data to save                 |
| `value`          | string        | Yes      | The value of the data to save               |
| `attributes`     | list          | No       | A list of `key`-`value` attributes useful for batch updating |

### Headers

| Name | Type | Description |
|----------|---------|--------------|
| `authentication-token` | string | Found in sign-in or sign-up responses. This token is used for user sessions |
| `Content-Type`         | string | Set it to `application/json` |

### Responses

| HTTP status code | content-type | Description |
|------------------|--------------|-------------|
| `200`              | application/json         | Object containing the user's attributes  |
| `400`              | text/plain | Missing or invalid parameters. Each `attribute` needs a `key` and `value' parameter |
| `500`              | text/plain | Unknown server error |

### Response object

| Attribute name                    | Type | Description |
|-----------------------------------|------|-------------|
| `game_user_attributes`            | list | All the users' attributes, i.e.: custom data |

### Examples

!!! example
    #### cURL

    ```shell
    curl --request POST \
        --header "authentication-token: abc123" \
        --header 'Content-Type: application/json' \
        --data '{"key": "some key","value": "some value"}'
        'https://gamefuse.co/api/v2/users/1/add_game_user_attribute'
    ```

    #### Response

    ```json
    {
      "game_user_attributes": [
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
          {
              "id": 2,
              "key": "some key",
              "value": "some value"
          }
      ]
    }
    ```

!!! example
    #### cURL

    ```shell
    curl --request POST \
        --header "authentication-token: abc123" \
        --header "Content-Type: application/json" \
        --data '{"key": "some key","value": "some value", "attributes": [{"key":"k0","value":"v0"},{"key":"k1","value":"v1"}]}' \
        'https://gamefuse.co/api/v2/users/1/add_game_user_attribute'
    ```

    #### Response

    ```json
    {
      "game_user_attributes": [
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
          {
              "id": 2,
              "key": "some key",
              "value": "some value"
          },
          {
              "id": 3,
              "key": "k0",
              "value": "v0"
          },
          {
              "id": 4,
              "key": "k1",
              "value": "v1"
          }
      ]
    }
    ```

## Removing a custom attribute 

### Scope

Remove an arbitrary custom attribute.

### Method

!!! info annotate "GET"
    ```plaintext
    /api/v2/users/{signedInUserId}/remove_game_user_attribute?game_user_attribute_key={gameUserAttributeKey}
    ```

### Attributes

| Name             | Type          | Required | Description |
|------------------|---------------|----------|-------------|
| `gameUserAttributeKey`           | string        | Yes      | The key of the data to remove |
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
| `200`              | application/json         | Object containing the user's attributes  |
| `400`              | text/plain | User does not have an item with the specified `key` |
| `500`              | text/plain | Unknown server error |

### Response object

| Attribute name                    | Type | Description |
|-----------------------------------|------|-------------|
| `game_user_attributes`            | list | All the users' attributes, i.e.: custom data |

### Examples

!!! example
    #### cURL

    ```shell
    curl --request GET \
        --header "authentication-token: abc123" \
        'https://gamefuse.co/api/v2/users/1/remove_game_user_attribute?game_user_attribute_key=some%20key'
    ```

    #### Example response

    ```json
    {
      "game_user_attributes": [
          {
              "id": 0,
              "key": "this_key",
              "value": "this_value"
          },
          {
              "id": 1,
              "key": "other_key",
              "value": "other_value"
          }
      ]
    }
    ```

## Get all custom attributes 

### Scope

Get all custom attributes of a user.

### Method

!!! info annotate "GET"
    ```plaintext
    /api/v2/users/{signedInUserId}/game_user_attributes
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
| `200             | application/json         | Object containing the user's attributes  |
| `500             | text/plain               | Unknown server error |

### Response object

| Attribute name                    | Type | Description |
|-----------------------------------|------|-------------|
| `game_user_attributes`            | list | All the users' attributes, i.e.: custom data |

### Examples

!!! example
    #### cURL

    ```shell
    curl --request GET \
        --header "authentication-token: abc123" \
        'https://gamefuse.co/api/v2/users/1/game_user_attributes'
    ```

    #### Example response

    ```json
    {
      "game_user_attributes": [
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
          {
              "id": 2,
              "key": "some key",
              "value": "some value"
          }
      ]
    }
    ```
