## Connecting To GameFuse

Once an account is created, you can verify your game and hit the connect endpoint to verfiy your game before using GameFuse enpoints with specific users:

```
https://gamefuse.co/api/v2/games/verify.json?game_id={game id}&game_token={api token}

```

It will have the following URL parameters:

```
game_id: {found on your GameFuse.co dashboard}
game_token: {API token found on your GameFuse.co dashboard}

```

This will return a response with the following json:

```
{
    "id": {DB unique id of the game},
    "name": {name of the game},
    "token": {api token of the game},
    "description": {game description},
    "game_variables": [
    {
        "id": {game_variable id 1},
        "key": {game_variable key 1},
        "value": {game_variable id 1}
    },
    {
        "id": {game_variable id 2},
        "key": {game_variable key 2},
        "value": {game_variable id 2}
    }...
    ]
}

```

```
* 401 - failed to verify game
* 500 - unknown server error

```

This is not a required step, but can be useful to verify a connection before logging in specific users

## Game Variables

Your Game Variables will be downloaded when you verify and connect with your game, but you can also re-fetch them whwnever you like.

```
https://gamefuse.co/api/v2/games/fetch_game_variables.json?game_id={game id}&game_token={api token}

```

It will have the following URL parameters:

```
game_id: {found on your GameFuse.co dashboard}
game_token: {API token found on your GameFuse.co dashboard}

```

This will return a response with the following json:

```
{
    "id": {DB unique id of the game},
    "name": {name of the game},
    "token": {api token of the game},
    "description": {game description},
    "game_variables": [
    {
        "id": {game_variable id 1},
        "key": {game_variable key 1},
        "value": {game_variable id 1}
    },
    {
        "id": {game_variable id 2},
        "key": {game_variable key 2},
        "value": {game_variable id 2}
    }...
    ]
}

```

```
* 404 - Failed to fetch game variables, check your token and id
* 500 - unknown server error
```