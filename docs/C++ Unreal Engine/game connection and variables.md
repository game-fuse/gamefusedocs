# Game Connection and Variables

## Connecting To GameFuse

The first step in using GameFuse after it is installed, and your account is
registered, is to run the `SetUpGame` function. After this you can run other
functions to register and sign-in other users, and read and write game data.

It is advisable to include the `SetUpGame` function in your GameMode to ensure
it is invoked when your game begins.

To import the GameFuse library, add this at the beginning of any script:

```cpp
#include "GameFuseManager.h"
```

GameFuseManager::SetUpGame should be called as soon as possible in your game setup. The best place to do this is from UGameInstance.

!!! example
    ```cpp
    void UMyGameInstance::BeginPlay()
    {
        Super::BeginPlay();
        
        // Get the GameFuse Manager subsystem
        UGameFuseManager* GameFuseManager = GetGameInstance()->GetSubsystem<UGameFuseManager>();
        
        // Set up the game connection
        FGFApiCallback OnGameSetup;
        OnGameSetup.AddLambda([this, GameFuseManager](const FGFAPIResponse& Response)
        {
            if(Response.bSuccess)
            {
                UE_LOG(LogTemp, Display, TEXT("Game setup successful"));
                UE_LOG(LogTemp, Display, TEXT("Response: %s"), *Response.ResponseStr);
                
                // Get game data from the subsystem
                const FGFGameData& GameData = GameFuseManager->GetGameData();
                UE_LOG(LogTemp, Display, TEXT("Game Name: %s"), *GameData.Name);
                UE_LOG(LogTemp, Display, TEXT("Game Description: %s"), *GameData.Description);
            }
            else
            {
                UE_LOG(LogTemp, Error, TEXT("Game setup failed"));
                UE_LOG(LogTemp, Error, TEXT("Response: %s"), *Response.ResponseStr);
            }
        });
        
        GameFuseManager->SetUpGame(YourGameId, YourGameToken, OnGameSetup);
    }
    ```

## Function Parameters

### `GameFuseManager->SetUpGame`

| Parameter | Type | Description |
|-----------|------|-------------|
| `GameId` | `int32` | The unique identifier of your game |
| `Token` | `FString` | The API token for your game |
| `Callback` | `FGFApiCallback` | Callback function to handle the response |

### Function return values

#### `GameFuseManager->SetUpGame`

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK - Game setup successful |
| `401`            | Failed to verify game - Check your Game ID and token |
| `500`            | Unknown server error |

## Game Variables

Your game variables will be downloaded when you verify and connect with your
game, but you can also re-fetch them whenever you like.

!!! example
    ```cpp
    void UMyObject::FetchGameVariables()
    {
        // Get the GameFuse Manager subsystem
        UGameFuseManager* GameFuseManager = GetGameInstance()->GetSubsystem<UGameFuseManager>();
        
        // Create a callback for the fetch operation
        FGFApiCallback CompletionCallback;
        CompletionCallback.AddLambda([this, GameFuseManager](const FGFAPIResponse& Response)
        {
            if(Response.bSuccess)
            {
                UE_LOG(LogTemp, Display, TEXT("Game variables fetched successfully"));
                UE_LOG(LogTemp, Display, TEXT("Response: %s"), *Response.ResponseStr);
                
                // Get the game variables from the subsystem
                TMap<FString, FString> GameVariables = GameFuseManager->GetGameVariables();
                
                // Access the variables
                for (const auto& Variable : GameVariables)
                {
                    UE_LOG(LogTemp, Display, TEXT("Variable: %s = %s"), *Variable.Key, *Variable.Value);
                }
            }
            else
            {
                UE_LOG(LogTemp, Error, TEXT("Failed to fetch game variables"));
                UE_LOG(LogTemp, Error, TEXT("Response: %s"), *Response.ResponseStr);
            }
        });
        
        // Fetch game variables
        GameFuseManager->FetchGameVariables(CompletionCallback);
    }
    ```

## Function Parameters

### `GameFuseManager->FetchGameVariables`

| Parameter | Type | Description |
|-----------|------|-------------|
| `Callback` | `FGFApiCallback` | Callback function to handle the response |

### Function return values

#### `GameFuseManager->FetchGameVariables`

| HTTP status code | Description |
|------------------|-------------|
| `200`              | OK - Game variables fetched successfully |
| `401`              | Failed to fetch game variables. Check your Game ID and token |
| `500`              | Unknown server error |
