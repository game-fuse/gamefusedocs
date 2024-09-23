Leaderboards can be easily created within GameFuse from the  Unreal Engine game
client. A leaderboard entry can be added with:

- `leaderboard_name`
- `score`
- `extra_attributes` (metadata)

for the current signed in user.

Leaderboards can be downloaded for a specific `leaderboard_name`, which would
gather and sort the high scores for all users in the game. Leaderboards can
also be downloaded for a specific user.

The example below shows submitting 2 leaderboard entries, then retrieving them
for the game, and for the current user.

!!! example
    ```cpp
    void UMyObject::AddLeaderboard()
    {
        FUserCallback CompletionCallback;
        CompletionCallback.BindDynamic(this, &UMyObject::OnAttributesFetchedCallback);

        UGameFuseUser* GameFuseUser = GetGameInstance()->GetSubsystem<UGameFuseUser> ();

        TMap < FString, FString > ExtraAttributes;
        ExtraAttributes.Add("deaths","15");
        ExtraAttributes.Add("Jewels","12");

        GameFuseUser->AddLeaderboardEntryWithAttributes("leaderboard_name", GameFuseUser->GetScore(), ExtraAttributes, CompletionCallback);

        // Adding Leaderboard without extra attributes.
        //GameFuseUser->AddLeaderboardEntry("leaderboard_name", GameFuseUser->GetScore(), CompletionCallback);
    }

    void UMyObject::OnLeaderboardAddedCallback(bool bSuccess, const FString& Response)
    {
        if(bSuccess)
        {
            UE_LOG(LogTemp, Display, TEXT("Game Connected Successfully"));
            UE_LOG(LogTemp, Display, TEXT("Result : %s"), *Response);

        }
        else
        {
            UE_LOG(LogTemp, Error, TEXT("Error connecting game"));
            UE_LOG(LogTemp, Error, TEXT("Result : %s"), *Response);
        }
    }

    void UMyObject::GetMyLeaderboards()
    {
        FUserCallback CompletionCallback;
        CompletionCallback.BindDynamic(this, &UMyObject::OnMyLeaderboardsFetchedCallback);

        UGameFuseUser* GameFuseUser = GetGameInstance()->GetSubsystem<UGameFuseUser> ();
        GameFuseUser->FetchMyLeaderboardEntries(12, false, CompletionCallback);
    }

    void UMyObject::OnMyLeaderboardsFetchedCallback(bool bSuccess, const FString& Response)
    {
        if(bSuccess)
        {
            UE_LOG(LogTemp, Display, TEXT("Game Connected Successfully"));
            UE_LOG(LogTemp, Display, TEXT("Result : %s"), *Response);

            UGameFuseUser* GameFuseUser = GetGameInstance()->GetSubsystem<UGameFuseUser> ();
            TArray < UGameFuseLeaderboardEntry* > MyLeaderboards = GameFuseUser->GetLeaderboards();

        }
        else
        {
            UE_LOG(LogTemp, Error, TEXT("Error connecting game"));
            UE_LOG(LogTemp, Error, TEXT("Result : %s"), *Response);
        }
    }

    void UMyObject::GetLeaderboards()
    {
        FManagerCallback CompletionCallback;
        CompletionCallback.BindDynamic(this, &UMyObject::OnLeaderboardsFetchedCallback);

        UGameFuseUser* GameFuseUser = GetGameInstance()->GetSubsystem<UGameFuseUser> ();

        UGameFuseManager::FetchLeaderboardEntries(GameFuseUser, 15, false, "leaderboard_name", CompletionCallback);
    }

    void UMyObject::OnLeaderboardsFetchedCallback(bool bSuccess, const FString& Response)
    {
        if(bSuccess)
        {
            UE_LOG(LogTemp, Display, TEXT("Game Connected Successfully"));
            UE_LOG(LogTemp, Display, TEXT("Result : %s"), *Response);

            TArray < UGameFuseLeaderboardEntry* > Leaderboards = UGameFuseManager::GetLeaderboard();
        }
        else
        {
            UE_LOG(LogTemp, Error, TEXT("Error connecting game"));
            UE_LOG(LogTemp, Error, TEXT("Result : %s"), *Response);
        }
    }
    ```

You can also clear all leaderboard entries in a specific leaderboard for the
current user like this:

!!! example
    ```cpp
    void UMyObject::ClearMyLeaderboards()
    {
        FUserCallback CompletionCallback;
        CompletionCallback.BindDynamic(this, &UMyObject::OnMyLeaderboardsClearedCallback);

        UGameFuseUser* GameFuseUser = GetGameInstance()->GetSubsystem<UGameFuseUser> ();
        GameFuseUser->ClearLeaderboardEntry("leaderboard_name", CompletionCallback);
    }

    void UMyObject::OnMyLeaderboardsClearedCallback(bool bSuccess, const FString& Response)
    {
        if(bSuccess)
        {
            UE_LOG(LogTemp, Display, TEXT("Game Connected Successfully"));
            UE_LOG(LogTemp, Display, TEXT("Result : %s"), *Response);
        }
        else
        {
            UE_LOG(LogTemp, Error, TEXT("Error connecting game"));
            UE_LOG(LogTemp, Error, TEXT("Result : %s"), *Response);
        }
    }
    ```

## Function return values

### `GameFuseUser->AddLeaderboardEntryWithAttributes`

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK |
| `400`            | Invalid extra attributes |
| `401`            | Can only add entries for current user |
| `500`            | Unknown server error |

### `UGameFuseManager::GetLeaderboard`

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK |
| `401`            | Can only get entries for the current user |
| `500`            | Unknown server error |

### `GameFuseUser->ClearLeaderboardEntry`

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK |
| `401`            | Can only clear entries for the current user |
| `500`            | Unknown server error |

### `UGameFuseManager::GetLeaderboard`

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK |
| `404`            | No entries for this leaderboard name |
| `500`            | Unknown server error |
