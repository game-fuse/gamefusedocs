# In-Game Leaderboard

Leaderboards can be easily created within GameFuse from the Unreal Engine game
client. A leaderboard entry can be added with:

- `leaderboard_name`
- `score`
- `extra_attributes` (metadata)

for the current signed in user.

Leaderboards can be downloaded for a specific `leaderboard_name`, which would
gather and sort the high scores for all users in the game. Leaderboards can
also be downloaded for a specific user.

## Adding Leaderboard Entries

The example below shows submitting leaderboard entries for the current user.

!!! example "C++ Example"
    ```cpp
    void UMyObject::AddLeaderboard()
    {
        // Get the GameFuse User subsystem
        UGameFuseUser* GameFuseUser = GetGameInstance()->GetSubsystem<UGameFuseUser>();
        
        // Create metadata for the leaderboard entry
        TMap<FString, FString> ExtraAttributes;
        ExtraAttributes.Add("deaths", "15");
        ExtraAttributes.Add("jewels", "12");
        
        // Create callback for the operation
        FGFInternalSuccessCallback CompletionCallback;
        CompletionCallback.BindLambda([this](bool bSuccess)
        {
            if(bSuccess)
            {
                UE_LOG(LogTemp, Display, TEXT("Leaderboard entry added successfully"));
            }
            else
            {
                UE_LOG(LogTemp, Error, TEXT("Failed to add leaderboard entry"));
            }
        });
        
        // Add leaderboard entry with metadata
        GameFuseUser->AddLeaderboardEntry("leaderboard_name", GameFuseUser->GetCurrentUserData().Score, ExtraAttributes, CompletionCallback);
        
        // Or add leaderboard entry without metadata
        // GameFuseUser->AddLeaderboardEntry("leaderboard_name", GameFuseUser->GetCurrentUserData().Score, CompletionCallback);
    }
    ```



## Fetching Leaderboard Entries

### Fetching Current User's Leaderboard Entries

!!! example "C++ Example"
    ```cpp
    void UMyObject::GetMyLeaderboards()
    {
        // Get the GameFuse User subsystem
        UGameFuseUser* GameFuseUser = GetGameInstance()->GetSubsystem<UGameFuseUser>();
        
        // Create callback for the operation
        FGFLeaderboardEntriesCallback CompletionCallback;
        CompletionCallback.BindLambda([this](bool bSuccess, const TArray<FGFLeaderboardEntry>& LeaderboardEntries)
        {
            if(bSuccess)
            {
                UE_LOG(LogTemp, Display, TEXT("Fetched %d leaderboard entries"), LeaderboardEntries.Num());
                
                for(const FGFLeaderboardEntry& Entry : LeaderboardEntries)
                {
                    UE_LOG(LogTemp, Display, TEXT("Leaderboard: %s, Score: %d, Username: %s"), 
                        *Entry.LeaderboardName, Entry.Score, *Entry.Username);
                }
            }
            else
            {
                UE_LOG(LogTemp, Error, TEXT("Failed to fetch leaderboard entries"));
            }
        });
        
        // Fetch current user's leaderboard entries
        GameFuseUser->FetchMyLeaderboardEntries(12, false, CompletionCallback);
    }
    ```

### Fetching Other Users' Leaderboard Entries

!!! example "C++ Example"
    ```cpp
    void UMyObject::GetUserLeaderboards(int32 UserId)
    {
        // Get the GameFuse User subsystem
        UGameFuseUser* GameFuseUser = GetGameInstance()->GetSubsystem<UGameFuseUser>();
        
        // Create callback for the operation
        FGFLeaderboardEntriesCallback CompletionCallback;
        CompletionCallback.BindLambda([this](bool bSuccess, const TArray<FGFLeaderboardEntry>& LeaderboardEntries)
        {
            if(bSuccess)
            {
                UE_LOG(LogTemp, Display, TEXT("Fetched %d leaderboard entries for user"), LeaderboardEntries.Num());
                
                for(const FGFLeaderboardEntry& Entry : LeaderboardEntries)
                {
                    UE_LOG(LogTemp, Display, TEXT("Leaderboard: %s, Score: %d, Username: %s"), 
                        *Entry.LeaderboardName, Entry.Score, *Entry.Username);
                }
            }
            else
            {
                UE_LOG(LogTemp, Error, TEXT("Failed to fetch user leaderboard entries"));
            }
        });
        
        // Fetch specific user's leaderboard entries
        GameFuseUser->FetchUserLeaderboardEntries(UserId, 12, false, CompletionCallback);
    }
    ```

### Fetching Global Leaderboard Entries

!!! example "C++ Example"
    ```cpp
    void UMyObject::GetGlobalLeaderboards()
    {
        // Get the GameFuse Manager subsystem
        UGameFuseManager* GameFuseManager = GetGameInstance()->GetSubsystem<UGameFuseManager>();
        
        // Create callback for the operation
        FGFApiCallback CompletionCallback;
        CompletionCallback.AddLambda([this, GameFuseManager](const FGFAPIResponse& Response)
        {
            if(Response.bSuccess)
            {
                UE_LOG(LogTemp, Display, TEXT("Global leaderboard entries fetched successfully"));
                
                // Get the leaderboard entries from the manager
                const TArray<FGFLeaderboardEntry>& Entries = GameFuseManager->GetLeaderboardEntries("leaderboard_name");
                
                for(const FGFLeaderboardEntry& Entry : Entries)
                {
                    UE_LOG(LogTemp, Display, TEXT("Leaderboard: %s, Score: %d, Username: %s"), 
                        *Entry.LeaderboardName, Entry.Score, *Entry.Username);
                }
            }
            else
            {
                UE_LOG(LogTemp, Error, TEXT("Failed to fetch global leaderboard entries: %s"), *Response.ResponseStr);
            }
        });
        
        // Fetch global leaderboard entries
        GameFuseManager->FetchLeaderboardEntries(15, false, "leaderboard_name", CompletionCallback);
    }
    ```

## Clearing Leaderboard Entries

You can clear all leaderboard entries in a specific leaderboard for the
current user:

!!! example "C++ Example"
    ```cpp
    void UMyObject::ClearMyLeaderboards()
    {
        // Get the GameFuse User subsystem
        UGameFuseUser* GameFuseUser = GetGameInstance()->GetSubsystem<UGameFuseUser>();
        
        // Create callback for the operation
        FGFInternalSuccessCallback CompletionCallback;
        CompletionCallback.BindLambda([this](bool bSuccess)
        {
            if(bSuccess)
            {
                UE_LOG(LogTemp, Display, TEXT("Leaderboard entries cleared successfully"));
            }
            else
            {
                UE_LOG(LogTemp, Error, TEXT("Failed to clear leaderboard entries"));
            }
        });
        
        // Clear current user's leaderboard entries
        GameFuseUser->ClearLeaderboardEntry("leaderboard_name", CompletionCallback);
    }
    ```


    }

    UFUNCTION(BlueprintCallable)
    void UMyObject::OnMyLeaderboardsClearedBlueprint(const FGFAPIResponse& Response)
    {
        if(Response.bSuccess)
        {
            UE_LOG(LogTemp, Display, TEXT("Leaderboard entries cleared successfully"));
        }
        else
        {
            UE_LOG(LogTemp, Error, TEXT("Failed to clear leaderboard entries: %s"), *Response.ResponseStr);
        }
    }
    ```

## Function Parameters

### Adding Leaderboard Entries

| Parameter | Type | Description |
|-----------|------|-------------|
| `LeaderboardName` | `FString` | The name of the leaderboard |
| `Score` | `int32` | The score to add |
| `Metadata` | `TMap<FString, FString>` | Optional metadata for the entry |
| `Callback` | `FGFInternalSuccessCallback` / `FBP_GFApiCallback` | Callback function to handle the response |

### Fetching Leaderboard Entries

| Parameter | Type | Description |
|-----------|------|-------------|
| `Limit` | `int32` | Maximum number of entries to fetch |
| `bOnePerUser` | `bool` | Whether to fetch only one entry per user |
| `LeaderboardName` | `FString` | The name of the leaderboard (for global fetch) |
| `UserId` | `int32` | The user ID to fetch entries for (for user-specific fetch) |
| `Callback` | `FGFLeaderboardEntriesCallback` / `FBP_GFApiCallback` | Callback function to handle the response |

## Function Return Values

### `GameFuseUser->AddLeaderboardEntry` / `GameFuseUser->BP_AddLeaderboardEntryWithAttributes`

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK - Entry added successfully |
| `400`            | Invalid extra attributes |
| `401`            | Can only add entries for current user |
| `500`            | Unknown server error |

### `GameFuseUser->FetchMyLeaderboardEntries` / `GameFuseUser->FetchUserLeaderboardEntries`

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK - Entries fetched successfully |
| `401`            | Unauthorized - User not signed in |
| `404`            | User not found (for user-specific fetch) |
| `500`            | Unknown server error |

### `GameFuseManager->FetchLeaderboardEntries`

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK - Entries fetched successfully |
| `404`            | No entries for this leaderboard name |
| `500`            | Unknown server error |

### `GameFuseUser->ClearLeaderboardEntry` / `GameFuseUser->BP_ClearLeaderboardEntry`

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK - Entries cleared successfully |
| `401`            | Can only clear entries for the current user |
| `500`            | Unknown server error |
