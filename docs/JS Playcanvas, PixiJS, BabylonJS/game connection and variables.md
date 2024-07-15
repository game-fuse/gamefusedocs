# Game Connection and Variables

## Connecting to GameFuse

The first step in using GameFuse after it is installed and your account is regestered is to run the SetUpGame function. After this step you can run other functions to register users, sign in users, read and write game data.

In any script on your first scene you can run:

```jsx
start () {
    var gameID = 'Your Game ID Here';
    var gameToken 'your Game Token Here';

    # 3rd param is the function below, GameFuse will call this function when it is done connecting your game
    let self = this;
    GameFuse.setUpGame(gameID, gameToken, function(message,hasError){self.gameSetUp(message,hasError)}, true);
}

gameSetUp(message, hasError) {
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

```
* 401 - failed to verify game
* 500 - unknown server error

```

## Game Variables

Your Game Variables will be downloaded when you verify and connect with your game, but you can also re-fetch them whwnever you like.

In any script run:

```jsx
void Start () {
    var gameID = 'Your Game ID Here';
    var gameToken 'your Game Token Here';

    # 3rd param is the function below, GameFuse will call this function when it is done connecting your game
    GameFuse.fetchGameVariables(gameID, gameToken, function(message,hasError){self.variablesFetched(message,hasError)});
}

void variablesFetched(string message, bool hasError) {
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

```
* 404 - Failed to fetch game variables, check your token and id
* 500 - unknown server error
```