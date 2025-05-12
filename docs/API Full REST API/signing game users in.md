After sign up, you should provide a sign in function for users to log-in at
a later time.

## Sign in users

### Scope

Make users log-in.

!!! important
    This step returns the `authentication-token` HTTP header which is to be
    used for most API requests.

!!! tip
    If you get a message like `Request missing user authentication token`
    from other API endpoints that require the `authentication-token` header,
    try connecting to this endpoint again and use the newly generated 
    `authentication_token` string.

### Method

!!! info annotate "POST"
    ```plaintext
    /api/v3/sessions
    ```

### Attributes

None

### Data (payload)

| Name             | Type          | Required | Description |
|------------------|---------------|----------|-------------|
| `email`          | string        | Yes      | User's email, used for login and forgot password functionality |
| `gameId`         | integer       | Yes      | Value found on your GameFuse.co dashboard |
| `gameToken`      | string        | Yes      | API token found on your GameFuse.co dashboard |
| `password`       | string        | Yes      | User's password |

### Headers

| Name | Type | Description |
|----------|---------|--------------|
| `Content-Type`         | string | Set it to `application/json` |

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
        --header 'Content-Type: application/json' \
        --data '{"email: "john.doe@example.com", "password": 1234", "password_confirmation": "1234", "username": "jdoe", "game_id": 1, "game_token": "abc123"}' \
        https://gamefuse.co/api/v3/sessions
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
        "game_sessions_current_month": 1,
        "game_user_attributes": "game_user_attributes": [
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
        ],
        "game_user_store_items": [
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
        ],
        "friends": [{
		  "id": 1,
		  "username": "john_doe",
		  "email": "john.doe@example.com",
		  "display_email": "john.doe@example.com",
		  "credits": 150,
		  "score": 1200
		},
        {
		  "id": 2,
		  "username": "dee_doo",
		  "email": "dee.doo@example.com",
		  "display_email": "dee.doo@example.com",
		  "credits": 150,
		  "score": 1200
		}],
        "outgoing_friend_requests": [
            {
            "id": 2,
            "username": "friend_username",
            "email": "friend@example.com",
            "display_email": "friend@example.com",
            "credits": 100,
            "score": 900,
            "friendship_id": 123,
            "requested_at": "2024-09-20T12:34:56Z"
            }
        ],
        "incoming_friend_requests": [
            {
            "id": 3,
            "username": "another_friend",
            "email": "another_friend@example.com",
            "display_email": "another_friend@example.com",
            "credits": 200,
            "score": 1300,
            "friendship_id": 124,
            "requested_at": "2024-09-19T11:22:33Z"
            }
        ],
        "groups": [
            {
            "id": 1,
            "name": "Awesome Gamers",
            "group_type": "Public",
            "can_auto_join": true,
            "is_invite_only": false,
            "max_group_size": 50,
            "searchable": true,
            "member_count": 12
            },
            {
            "id": 2,
            "name": "VIP Squad",
            "group_type": "Private",
            "can_auto_join": false,
            "is_invite_only": true,
            "max_group_size": 10,
            "searchable": false,
            "member_count": 5
            }
        ],
        "group_join_requests": [],
        "group_invites": [{
            "invite": [],
            "user": {
            "id": 3,
            "username": "me",
            "email": "me@example.com",
            "display_email": "me@example.com",
            "credits": 200,
            "score": 1300,
            "requested_at": "2024-09-19T11:22:33Z"
            },
            "inviter": {
            "id": 3,
            "username": "inviter",
            "email": "inviter@example.com",
            "display_email": "inviter@example.com",
            "credits": 100,
            "score": 200,
            "requested_at": "2024-09-19T11:22:33Z"
            },
            "group": {
                "id": 1,
                "name": "Awesome Gamers",
                "group_type": "Public",
                "can_auto_join": true,
                "is_invite_only": false,
                "max_group_size": 50,
                "searchable": true,
                "member_count": 1,
                "members": [
                    {
                    "id": 10,
                    "username": "john_doe",
                    "email": "john.doe@example.com",
                    "credits": 150,
                    "score": 1200
                    }
                ],
                "admins": [
                    {
                    "id": 10,
                    "username": "john_doe",
                    "email": "john.doe@example.com",
                    "credits": 150,
                    "score": 1200
                    }
                ]
            }

        }]
    }
    ```
