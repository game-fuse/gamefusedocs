# Game Connection and Variables

## Connecting to GameFuse

The first step in using GameFuse after it is installed, and your account is
registered, is to run the `SetUpGame` function. After this you can run other
functions to register and sign-in other users, and read and write game data.

Inside any script, on your first scene, you can run:

!!! example
    ```jsx
    start ()
    {
        var gameID = '1';
        var gameToken 'cde456';

        // The third parameter is the function below.
        // GameFuse calls this function when it completes setting up your game.
        let self = this;
        GameFuse.setUpGame(gameID, gameToken, function(message,hasError){self.gameSetUp(message,hasError)}, true);
    }

    gameSetUp(message, hasError)
    {
        if (hasError)
        {
            console.log("Error connecting game: "+message);
        }
        else
        {
            console.log("Game Connected Successfully")
            foreach (GameFuseStoreItem storeItem in GameFuse.GetStoreItems())
            {
                console.log(storeItem.getName() + ": " + storeItem.getCost());
            }
        }
    }
    ```

### Function return values

#### `GameFuse.setUpGame`

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK |
| `401`            | Failed to verify game |
| `500`            | Unknown server error |

## Game Variables

Your game variables will be downloaded when you verify and connect with you
game, but you can also re-fetch them whenever you like.

Inside any script use these functions:

!!! example
    ```jsx
    void Start ()
    {
        var gameID = '1';
        var gameToken 'cde456';

        // The third parameter is the function below.
        // GameFuse calls this function when it completes setting up your game.
        GameFuse.fetchGameVariables(gameID, gameToken, function(message,hasError){self.variablesFetched(message,hasError)});
    }

    void variablesFetched(string message, bool hasError)
    {
        if (hasError)
        {
            console.log("Error connecting game: "+message);
        }
        else
        {
            console.log("Game Connected Successfully")
            console.log(`Game Variables: ${GameFuse.Instance.GetGameVariable({your key})}`)
        }
    }
```

### Function return values

#### `GameFuse.fetchGameVariables`

| HTTP status code | Description |
|------------------|-------------|
| `200`              | OK |
| `401`              | Failed to fetch game variables. Check your Game ID and token |
| `500`              | Unknown server error |
