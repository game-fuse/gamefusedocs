# Connecting To GameFuse

The initial action after installing GameFuse and registering your account is to execute the 'SetUpGame' function. Following this step, you can proceed to call other functions for user registration, user sign-in, and reading or writing game data. It's advisable to include the 'SetUpGame' function in your GameMode to ensure it's invoked when your game begins.

Make sure to include this header to enable the utilization of GameFuse functions:

```cpp
#include "GameFuseManager.h"
```

In any script on your first scene you can run:

```cpp
void AMyGameModeBase::Start()
{
  const FString GameId = "{your-game-id}";
  const FString Token = "{your-game-token}";
  constexpr bool bSeedStore = false;

  FManagerCallback CompletionCallback;
  CompletionCallback.BindDynamic(this, &AMyGameModeBase::GameSetUpCallback);

  UGameFuseManager::SetUpGame(GameId, Token, bSeedStore, CompletionCallback);
}

void AMyGameModeBase::GameSetUpCallback(bool bSuccess, const FString& Response)
{
  if(bSuccess)
  {
  	UE_LOG(LogTemp, Display, TEXT("SetUp Game Success"));
  	UE_LOG(LogTemp, Display, TEXT("Result : %s"), *Response);
  }else
  {
  	UE_LOG(LogTemp, Error, TEXT("SetUp Game Failed"));
  	UE_LOG(LogTemp, Error, TEXT("Result : %s"), *Response);
  }
}

```

```
* 401 - failed to verify game
* 500 - unknown server error

```

# Game Variables

Your Game Variables will be downloaded when you verify and connect with your game, but you can also re-fetch them whwnever you like.

```cpp
void UMyObject::Start()
{
  FManagerCallback CompletionCallback;
  CompletionCallback.BindDynamic(this, &UMyObject::VariablesFetchedCallback);

  UGameFuseManager::FetchGameVariables(CompletionCallback);
}

void UMyObject::VariablesFetchedCallback(bool bSuccess, const FString& Response)
{
  if(bSuccess)
  {
    UE_LOG(LogTemp, Display, TEXT("Game Connected Successfully"));
    UE_LOG(LogTemp, Display, TEXT("Result : %s"), *Response);

    TMap < FString, FString > OurVariables = UGameFuseManager::GetGameVariables();

  }else
  {
    UE_LOG(LogTemp, Error, TEXT("Error connecting game"));
    UE_LOG(LogTemp, Error, TEXT("Result : %s"), *Response);
  }
}

```

```
* 404 - Failed to fetch game variables, check your token and id
* 500 - unknown server error
```