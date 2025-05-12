
# Fetching Users details


Retrieve details about a specific user. This can be used to fetch information about the signed-in user or another user in the same game.

### Method

!!! info annotate "GET"
    ```plaintext
    /api/v3/users/{id}
    ```

!!! tip annotate "NOTE"
    ```plaintext
    Other users in the same game can be queried with this route.
    ```

### Attributes

| Name | Type    | Required | Description                                      |
|------|---------|----------|--------------------------------------------------|
| `id` | integer | Yes      | The ID of the user to retrieve details for.      |

### Data (payload)

None

### Headers

| Name                  | Type   | Description                                   |
|-----------------------|--------|-----------------------------------------------|
| `authentication-token` | string | Found in sign-in or sign-up responses. This token is used for user sessions. |

### Responses

| HTTP status code | content-type    | Description                                              |
|------------------|-----------------|----------------------------------------------------------|
| `200`            | application/json| User details retrieved successfully.                     |
| `401`            | text/plain      | Unauthorized: You cannot view users from another game.   |
| `404`            | text/plain      | User not found.                                          |
| `500`            | text/plain      | Unknown server error.                                    |

### Response object

| Attribute name | Type    | Description                                      |
|----------------|---------|--------------------------------------------------|
| `id`           | integer | The ID of the user.                              |
| `username`     | string  | The username of the user.                        |
| `email`        | string  | The email of the user.                           |
| `credits`      | integer | The number of credits the user has.              |
| `score`        | integer | The score of the user.                           |
| `is_new_user`  | boolean | Indicates if the user is new.                    |
| `last_login`   | string  | The last login date of the user.                 |
| `number_of_logins` | integer | The total number of logins by the user.       |

### Examples

!!! example 1
    #### cURL

    ```shell
    curl --request GET \
        --header "authentication-token: abc123" \
        "https://gamefuse.co/api/v3/users/1"
    ```

    #### Response

    ```json
    {
        "id": 1,
        "username": "john.doe",
        "email": "john.doe@example.com",
        "credits": 1000,
        "score": 1500,
        "is_new_user": false,
        "last_login": "2024-07-23",
        "number_of_logins": 42
    }
    ```



# Fetching Other User's Data


After sign in, a signed in user will have an authentication-token that can be placed in the header of subsequent API calls.  Now the user can create, read, update and destroy their data in any of the GameFuse constructs.  LeaderboardEntries, GameRounds, PurchasedStoreItems, GameUserAttributes, Friends, Groups, Messages and more can be accessed to keep track of your user's game state.

In accessing leaderboards, groups, multiplayer game rounds and more, you may want to query other users in those collections that are not signed in. Other users' data cannot be created or modified, but it can be read to display to the signed-in user, perhaps showing a competitor's avatar image URL on a leaderboard or on a friend's chat message window.

Below is a list of accessible routes that a signed-in user can query other users' data with. The ID of the user passed in must not be the signed-in user; it must be the user being queried.  


### API Paths

| Method | Path                                           | Description                              |
|--------|------------------------------------------------|------------------------------------------|
| GET    | `/v3/users/:id/game_user_attributes`           | Fetch game user attributes for a user    |
| GET    | `/v3/users/:id/game_user_store_items`          | Fetch store items for a user             |
| GET    | `/v3/users/:id/leaderboard_entries`            | Fetch leaderboard entries for a user     |
| GET    | `/ve/users/:id`                                | Fetch details of a user (show route)     |
| GET    | `/v3/friends?user_id=xxx`                      | Fetch the list of friends                |
| GET    | `/v3/game_rounds?user_id=xxx`                  | Fetch all game rounds                    |
| GET    | `/v3/game_rounds/:id`                          | Fetch details of a specific game round   |
| GET    | `/v3/game_rounds/multiplayer_game_round?id=xxx`| Fetch multiplayer game round details     |



Detail about these paths and expected responses can be found in the subsequent chapters. To access the user object for yourself of another user, use the following method:


