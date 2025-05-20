# Service Keys

### Intro
Accessing the GameFuse API via a 3rd party server such as a multiplayer server is easy.  It is reccomended to make manual API calls manually rather than use any of GameFuse's libraries, those were designed for implementation in the game client itself and are archetected around user integration, not 3rd party server integration.  

### Usage
In your 3rd party server code, you can hit any API endpoint "on behalf" of any user in your system.  You can for instance update a users credits, add a LeaderboardEntry, add a GameRound or more on behalf of certain users.  The API takes in addition to the standard user token authentication, also a "service key" authentication. An easy example to understand this is when a multiplayer round is done, the multiplayer server itself submits all the players scores to create new LeaderboardEntries on GameFuse. This frees up the game client from having to implement this, and prevents hacking since the user's devices are no longer sending HTTP requests that could be modified with improved scores.

### Enabling
First, navigate to your game on the https://gamefuse.co/games page, then click on the right-most tab "Service Keys".  Once that page has loaded you can create a new service key to use, it never expires unless you delete the key.

### Usage
On your 3rd party server you can now call any request in the API but instead of having an 'authentication-token' header, you can add 'service-key-name', 'service-key-token', and 'current-user-id', and the API will process the appropriate action on behalf of the user indicated by 'current-user-id'.

### Example

!!! example
    #### cURL

    ```shell
    curl --request POST \
        --header "service-key-name: photon-key" \
        --header "service-key-token: zdflkjd-sf38sfl-394lkejs-dfj209" \
        --header "current-user-id: 1" \
        --header "Content-Type: application/json" \
        --data '{"score": 21, "leaderboard_name": "leaderboard test", "metadata": {"level": "10", "color": "blue"}}' \
        "https://gamefuse.co/api/v3/users/1/add_leaderboard_entry"
    ```

    #### Response
    Upon success user object is now returned with code 200 - indicating the leaderboard entry has been saved

    ```json
    {
        "id": 1,
        "username": "john.doe",
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


### Route Enabling
On the same page as you create new service keys, you can also enable and disable routes.  Check and uncheck the boxes from each row to enable/disable the route via "Game User Access" or "Service Key Access".  This enables developers to prevent client hacking.  If you disable the Game User Access for all leaderboard routes for instance, game clients will be locked out of updating any leaderboard entries.  In this case only the 3rd party server will be able to update the leaderboard, preventing game clients from faking HTTP requests and manipulating data on their own.
