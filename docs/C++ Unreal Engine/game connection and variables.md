## Connecting To GameFuse

The first step in using GameFuse after it is installed, and your account is
registered, is to run the `SetUpGame` function. After this you can run other
functions to register and sign-in other users, and read and write game data.

It is advisable to include the `SetUpGame` function in your GameMode to ensure
it is invoked when your game begins.

To import the GameFuse library, add this at the beginning of any script:

```cpp
#include "GameFuseCore.h"
```

Inside any script, on your first scene, you can run:

!!! example
    ```cpp
    void AMyGameModeBase::Start()
    {
        const FString GameId = "1";
        const FString Token = "cde456";
        constexpr bool bSeedStore = false;

        FManagerCallback CompletionCallback;
        CompletionCallback.BindDynamic(this, &AMyGameModeBase::GameSetUpCallback);

        UGameFuseCore::SetUpGame(GameId, Token, bSeedStore, CompletionCallback);
    }

    void AMyGameModeBase::GameSetUpCallback(bool bSuccess, const FString& Response)
    {
        if(bSuccess)
        {
            UE_LOG(LogTemp, Display, TEXT("SetUp Game Success"));
            UE_LOG(LogTemp, Display, TEXT("Result : %s"), *Response);
        }
        else
        {
            UE_LOG(LogTemp, Error, TEXT("SetUp Game Failed"));
            UE_LOG(LogTemp, Error, TEXT("Result : %s"), *Response);
        }
    }
    ```

### Function return values

#### `UGameFuseCore::SetUpGame`

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK |
| `401`            | Failed to verify game |
| `500`            | Unknown server error |

## Game Variables

Your game variables will be downloaded when you verify and connect with you
game, but you can also re-fetch them whenever you like.

!!! example
    ```cpp
    void UMyObject::Start()
    {
        FManagerCallback CompletionCallback;
        CompletionCallback.BindDynamic(this, &UMyObject::VariablesFetchedCallback);

        UGameFuseCore::FetchGameVariables(CompletionCallback);
    }

    void UMyObject::VariablesFetchedCallback(bool bSuccess, const FString& Response)
    {
        if(bSuccess)
        {
            UE_LOG(LogTemp, Display, TEXT("Game Connected Successfully"));
            UE_LOG(LogTemp, Display, TEXT("Result : %s"), *Response);

            TMap < FString, FString > OurVariables = UGameFuseCore::GetGameVariables();
        }
        else
        {
            UE_LOG(LogTemp, Error, TEXT("Error connecting game"));
            UE_LOG(LogTemp, Error, TEXT("Result : %s"), *Response);
        }
    }
    ```

### Function return values

#### `UGameFuseCore::FetchGameVariables`

| HTTP status code | Description |
|------------------|-------------|
| `200`              | OK |
| `401`              | Failed to fetch game variables. Check your Game ID and token |
| `500`              | Unknown server error |
