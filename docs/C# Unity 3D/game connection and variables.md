## Connecting To GameFuse

The first step in using GameFuse after it is installed, and your account is
registered, is to run the `SetUpGame` function. After this you can run other
functions to register and sign-in other users, and read and write game data.

If you want import the GameFuse library, add this at the beginning of
any script:

```csharp
using GameFuseCSharp;
```

Inside any script, on your first scene, you can run:

!!! example
    ```csharp
    void Start () {
        var gameID = '1';
        var gameToken 'cde456';

        // The third parameter is the callback function below.
        // GameFuse calls this function when it completes setting up your game.
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

### Function return values

#### `GameFuse.SetUpGame`

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK |
| `401`            | Failed to verify game |
| `500`            ! Unknown server error |

## Game Variables

Your game variables will be downloaded when you verify and connect with you
game, but you can also re-fetch them whewnever you like.

Inside any script use these functions:

!!! example
    ```csharp
    void Start () {
        var gameID = '1';
        var gameToken 'cde456';

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

### Function return values

#### `GameFuse.FetchGameVariables`

| HTTP status code | Description |
|------------------|-------------|
| `200`              | OK |
| `401`              | Failed to fetch game variables. Check your Game ID and token |
| `500`              | Unknown server error |
