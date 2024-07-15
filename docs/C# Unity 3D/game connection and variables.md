## Connecting To GameFuse

The first step in using GameFuse after it is installed and your account is regestered is to run the SetUpGame function. After this step you can run other functions to register users, sign in users, read and write game data.

At the top of any script you want to use the GameFuse library in, you will need to do:

```csharp
using GameFuseCSharp;

```

In any script on your first scene you can run:

```csharp
void Start () {
    var gameID = 'Your Game ID Here';
    var gameToken 'your Game Token Here';

    # 3rd param is the function below, GameFuse will call this function when it is done connecting your game
    GameFuse.SetUpGame(gameToken, gameID, GameSetUpCallback);
}

void GameSetUpCallback(string message, bool hasError) {
    if (hasError)
    {
        Debug.Log("Error connecting game: "+message);
    }
    else
    {
        Debug.Log("Game Connected Successfully")
        foreach (GameFuseStoreItem storeItem in GameFuse.GetStoreItems())
        {
            Debug.Log(storeItem.GetName() + ": " + storeItem.GetCost());
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

```csharp
void Start () {
    var gameID = 'Your Game ID Here';
    var gameToken 'your Game Token Here';

    # 3rd param is the function below, GameFuse will call this function when it is done connecting your game
    GameFuse.FetchGameVariables(gameID, gameToken, VariablesFetched);
}

void VariablesFetched(string message, bool hasError) {
    if (hasError)
    {
        Debug.Log("Error connecting game: "+message);
    }
    else
    {
        Debug.Log("Game Connected Successfully")
    }
}

```

```
* 404 - Failed to fetch game variables, check your token and id
* 500 - unknown server error
```