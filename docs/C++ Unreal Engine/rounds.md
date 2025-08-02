# Rounds System

The GameFuse Rounds System allows you to track and manage game rounds in your game. This includes creating rounds, fetching round data, and managing round metadata.

## Getting Started with Rounds

To use the GameFuse Rounds system, you'll need to access the `UGameFuseRounds` subsystem:

```cpp
UGameFuseRounds* RoundsSystem = GetGameInstance()->GetSubsystem<UGameFuseRounds>();
```

## Creating a Game Round

You can create a new game round with specific settings:

!!! example "C++ Example"
    ```cpp
    void UMyGameMode::CreateGameRound()
    {
        UGameFuseRounds* RoundsSystem = GetGameInstance()->GetSubsystem<UGameFuseRounds>();
        
        // Create a new game round structure
        FGFGameRound RoundData;
        RoundData.Score = 100;
        RoundData.StartTime = FDateTime::Now();
        RoundData.EndTime = RoundData.StartTime + FTimespan::FromMinutes(1);
        RoundData.GameType = "SinglePlayer";
        
        // Create typed callback
        FGFGameRoundCallback OnCreateGameRound;
        OnCreateGameRound.BindLambda([this](const FGFGameRound& CreatedRound) {
            UE_LOG(LogTemp, Display, TEXT("Game round created successfully"));
            UE_LOG(LogTemp, Display, TEXT("Round ID: %d"), CreatedRound.Id);
            UE_LOG(LogTemp, Display, TEXT("Score: %d"), CreatedRound.Score);
            UE_LOG(LogTemp, Display, TEXT("Game Type: %s"), *CreatedRound.GameType);
        });
        
        // Create the game round
        RoundsSystem->CreateGameRound(RoundData, OnCreateGameRound);
    }
    ```



## Creating a Game Round with Metadata

You can include additional metadata with your game round:

!!! example "C++ Example"
    ```cpp
    void UMyGameMode::CreateGameRoundWithMetadata()
    {
        UGameFuseRounds* RoundsSystem = GetGameInstance()->GetSubsystem<UGameFuseRounds>();
        
        // Create a new game round structure
        FGFGameRound RoundData;
        RoundData.Score = 100;
        RoundData.StartTime = FDateTime::Now();
        RoundData.EndTime = RoundData.StartTime + FTimespan::FromMinutes(1);
        RoundData.GameType = "SinglePlayer";
        
        // Add metadata
        RoundData.Metadata.Add("key1", "value1");
        RoundData.Metadata.Add("key2", "value2");
        RoundData.Metadata.Add("difficulty", "hard");
        RoundData.Metadata.Add("level", "5");
        
        // Create typed callback
        FGFGameRoundCallback OnCreateGameRoundWithMetadata;
        OnCreateGameRoundWithMetadata.BindLambda([this](const FGFGameRound& CreatedRound) {
            UE_LOG(LogTemp, Display, TEXT("Game round with metadata created successfully"));
            UE_LOG(LogTemp, Display, TEXT("Round ID: %d"), CreatedRound.Id);
            
            // Access metadata
            for (const auto& Pair : CreatedRound.Metadata)
            {
                UE_LOG(LogTemp, Display, TEXT("Metadata - %s: %s"), *Pair.Key, *Pair.Value);
            }
        });
        
        // Create the game round with metadata
        RoundsSystem->CreateGameRound(RoundData, OnCreateGameRoundWithMetadata);
    }
    ```

## Creating a Multiplayer Game Round

You can create a multiplayer game round by specifying the game type and including player IDs:

!!! example "C++ Example"
    ```cpp
    void UMyGameMode::CreateMultiplayerGameRound()
    {
        UGameFuseRounds* RoundsSystem = GetGameInstance()->GetSubsystem<UGameFuseRounds>();
        
        // Create a new game round structure
        FGFGameRound RoundData;
        RoundData.Score = 200;
        RoundData.StartTime = FDateTime::Now();
        RoundData.EndTime = RoundData.StartTime + FTimespan::FromMinutes(5);
        RoundData.GameType = "Multiplayer";
        RoundData.bMultiplayer = true;
        
        // Add metadata
        RoundData.Metadata.Add("map", "desert");
        RoundData.Metadata.Add("mode", "capture_the_flag");
        
        // Create typed callback
        FGFGameRoundCallback OnCreateMultiplayerGameRound;
        OnCreateMultiplayerGameRound.BindLambda([this](const FGFGameRound& CreatedRound) {
            UE_LOG(LogTemp, Display, TEXT("Multiplayer game round created successfully"));
            UE_LOG(LogTemp, Display, TEXT("Round ID: %d"), CreatedRound.Id);
            UE_LOG(LogTemp, Display, TEXT("Multiplayer: %s"), CreatedRound.bMultiplayer ? TEXT("Yes") : TEXT("No"));
        });
        
        // Create the multiplayer game round
        RoundsSystem->CreateGameRound(RoundData, OnCreateMultiplayerGameRound);
    }
    ```

## Fetching a Game Round

You can fetch a specific game round by its ID:

!!! example "C++ Example"
    ```cpp
    void UMyGameMode::FetchGameRound(int32 RoundId)
    {
        UGameFuseRounds* RoundsSystem = GetGameInstance()->GetSubsystem<UGameFuseRounds>();
        
        // Create typed callback
        FGFGameRoundCallback OnFetchGameRound;
        OnFetchGameRound.BindLambda([this](const FGFGameRound& Round) {
            UE_LOG(LogTemp, Display, TEXT("Game round fetched successfully"));
            UE_LOG(LogTemp, Display, TEXT("Round ID: %d"), Round.Id);
            UE_LOG(LogTemp, Display, TEXT("Score: %d"), Round.Score);
            UE_LOG(LogTemp, Display, TEXT("Game Type: %s"), *Round.GameType);
            
            // Access metadata
            for (const auto& Pair : Round.Metadata)
            {
                UE_LOG(LogTemp, Display, TEXT("Metadata - %s: %s"), *Pair.Key, *Pair.Value);
            }
        });
        
        // Fetch the game round
        RoundsSystem->FetchGameRound(RoundId, OnFetchGameRound);
    }
    ```

## Fetching Game Rounds

### Fetching Current User's Game Rounds

!!! example "C++ Example"
    ```cpp
    void UMyGameMode::FetchMyGameRounds()
    {
        UGameFuseRounds* RoundsSystem = GetGameInstance()->GetSubsystem<UGameFuseRounds>();
        
        // Create typed callback
        FGFGameRoundListCallback OnFetchMyGameRounds;
        OnFetchMyGameRounds.BindLambda([this](const TArray<FGFGameRound>& Rounds) {
            UE_LOG(LogTemp, Display, TEXT("Fetched %d game rounds"), Rounds.Num());
            
            for (const FGFGameRound& Round : Rounds)
            {
                UE_LOG(LogTemp, Display, TEXT("Round ID: %d, Score: %d, Type: %s"), 
                    Round.Id, Round.Score, *Round.GameType);
            }
        });
        
        // Fetch current user's game rounds with pagination and filtering
        RoundsSystem->FetchMyGameRounds(OnFetchMyGameRounds, "SinglePlayer", 0, 10);
    }
    ```



### Fetching Other Users' Game Rounds

!!! example "C++ Example"
    ```cpp
    void UMyGameMode::FetchUserGameRounds(int32 UserId)
    {
        UGameFuseRounds* RoundsSystem = GetGameInstance()->GetSubsystem<UGameFuseRounds>();
        
        // Create typed callback
        FGFGameRoundListCallback OnFetchUserGameRounds;
        OnFetchUserGameRounds.BindLambda([this](const TArray<FGFGameRound>& Rounds) {
            UE_LOG(LogTemp, Display, TEXT("Fetched %d game rounds for user"), Rounds.Num());
            
            for (const FGFGameRound& Round : Rounds)
            {
                UE_LOG(LogTemp, Display, TEXT("Round ID: %d, Score: %d, Type: %s"), 
                    Round.Id, Round.Score, *Round.GameType);
            }
        });
        
        // Fetch specific user's game rounds with pagination and filtering
        RoundsSystem->FetchUserGameRounds(UserId, OnFetchUserGameRounds, "Multiplayer", 0, 10);
    }
    ```



## Updating a Game Round

You can update an existing game round:

!!! example "C++ Example"
    ```cpp
    void UMyGameMode::UpdateGameRound(int32 RoundId)
    {
        UGameFuseRounds* RoundsSystem = GetGameInstance()->GetSubsystem<UGameFuseRounds>();
        
        // First fetch the existing round
        FGFGameRoundCallback OnFetchForUpdate;
        OnFetchForUpdate.BindLambda([this, RoundsSystem, RoundId](const FGFGameRound& ExistingRound) {
            // Create an updated version of the round
            FGFGameRound UpdatedRound = ExistingRound;
            UpdatedRound.Score = 150; // Update the score
            UpdatedRound.EndTime = FDateTime::Now(); // Update the end time
            
            // Add or update metadata
            UpdatedRound.Metadata.Add("completed", "true");
            
            // Create callback for the update operation
            FGFGameRoundCallback OnUpdateGameRound;
            OnUpdateGameRound.BindLambda([this](const FGFGameRound& UpdatedRoundResult) {
                UE_LOG(LogTemp, Display, TEXT("Game round updated successfully"));
                UE_LOG(LogTemp, Display, TEXT("Updated Score: %d"), UpdatedRoundResult.Score);
            });
            
            // Update the game round
            RoundsSystem->UpdateGameRound(RoundId, UpdatedRound, OnUpdateGameRound);
        });
        
        // First fetch the existing round
        RoundsSystem->FetchGameRound(RoundId, OnFetchForUpdate);
    }
    ```

## Deleting a Game Round

You can delete a game round:

!!! example "C++ Example"
    ```cpp
    void UMyGameMode::DeleteGameRound(int32 RoundId)
    {
        UGameFuseRounds* RoundsSystem = GetGameInstance()->GetSubsystem<UGameFuseRounds>();
        
        // Create typed callback
        FGFGameRoundActionCallback OnDeleteGameRound;
        OnDeleteGameRound.BindLambda([this](bool bSuccess) {
            if (bSuccess)
            {
                UE_LOG(LogTemp, Display, TEXT("Game round deleted successfully"));
            }
            else
            {
                UE_LOG(LogTemp, Error, TEXT("Failed to delete game round"));
            }
        });
        
        // Delete the game round
        RoundsSystem->DeleteGameRound(RoundId, OnDeleteGameRound);
    }
    ```



## Function Parameters

### Creating and Updating Game Rounds

| Parameter | Type | Description |
|-----------|------|-------------|
| `RoundData` | `FGFGameRound` | The game round data to create or update |
| `RoundId` | `int32` | The ID of the round to update or delete |
| `Callback` | Various callback types | Callback function to handle the response |

### Fetching Game Rounds

| Parameter | Type | Description |
|-----------|------|-------------|
| `UserId` | `int32` | The user ID to fetch rounds for (optional, for user-specific fetch) |
| `GameType` | `FString` | Optional game type filter (e.g., "SinglePlayer", "Multiplayer") |
| `Page` | `int32` | Page number for pagination (0-based) |
| `PerPage` | `int32` | Number of rounds per page |
| `Callback` | `FGFGameRoundListCallback` | Callback function to handle the response |

## Function Return Values

### Game Round Operations

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK - Operation successful |
| `400`            | Bad request - Invalid parameters |
| `401`            | Unauthorized - User not signed in |
| `404`            | Game round not found |
| `500`            | Unknown server error |

### Fetching Game Rounds

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK - Game rounds fetched successfully |
| `401`            | Unauthorized - User not signed in |
| `404`            | User not found (for user-specific fetch) |
| `500`            | Unknown server error |

## Cached Data Access

You can access cached rounds data without making API calls:

```cpp
UGameFuseRounds* RoundsSystem = GetGameInstance()->GetSubsystem<UGameFuseRounds>();

// Get cached user rounds
const TArray<FGFGameRound>& UserRounds = RoundsSystem->GetUserRounds();

// Clear cached data if needed
RoundsSystem->ClearRoundsData();
```