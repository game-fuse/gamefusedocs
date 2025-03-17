# Rounds System

The GameFuse Rounds System allows you to track and manage game rounds in your game. This includes creating rounds, fetching round data, and managing round metadata.

## Getting Started with Rounds

To use the GameFuse Rounds system, you'll need to access the `UGameFuseRounds` subsystem:

```cpp
UGameFuseRounds* RoundsSystem = GetGameInstance()->GetSubsystem<UGameFuseRounds>();
```

## Creating a Game Round

You can create a new game round with specific settings:

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
        // Use CreatedRound data here (ID, score, game type, etc.)
    });
    
    // Create the game round
    RoundsSystem->CreateGameRound(RoundData, OnCreateGameRound);
}
```

## Creating a Game Round with Metadata

You can include additional metadata with your game round:

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
        // Use CreatedRound data and metadata here
        // Access metadata with CreatedRound.Metadata["key"]
    });
    
    // Create the game round with metadata
    RoundsSystem->CreateGameRound(RoundData, OnCreateGameRoundWithMetadata);
}
```

## Creating a Multiplayer Game Round

You can create a multiplayer game round by specifying the game type and including player IDs:

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
    
    // Add metadata
    RoundData.Metadata.Add("map", "desert");
    RoundData.Metadata.Add("mode", "capture_the_flag");
    
    // Create typed callback
    FGFGameRoundCallback OnCreateMultiplayerGameRound;
    OnCreateMultiplayerGameRound.BindLambda([this](const FGFGameRound& CreatedRound) {
        // Use CreatedRound data for multiplayer setup
        // Check player IDs with CreatedRound.PlayerIds
    });
    
    // Create the multiplayer game round
    RoundsSystem->CreateGameRound(RoundData, OnCreateMultiplayerGameRound);
}
```

## Fetching a Game Round

You can fetch a specific game round by its ID:

```cpp
void UMyGameMode::FetchGameRound(int32 RoundId)
{
    UGameFuseRounds* RoundsSystem = GetGameInstance()->GetSubsystem<UGameFuseRounds>();
    
    // Create typed callback
    FGFGameRoundCallback OnFetchGameRound;
    OnFetchGameRound.BindLambda([this](const FGFGameRound& Round) {
        // Use Round data to display round details
        // Access metadata with Round.Metadata
    });
    
    // Fetch the game round
    RoundsSystem->FetchGameRound(RoundId, OnFetchGameRound);
}
```

## Fetching User Game Rounds

You can fetch all game rounds for the current user:

```cpp
void UMyGameMode::FetchUserGameRounds()
{
    UGameFuseRounds* RoundsSystem = GetGameInstance()->GetSubsystem<UGameFuseRounds>();
    
    // Create typed callback
    FGFGameRoundsCallback OnFetchUserGameRounds;
    OnFetchUserGameRounds.BindLambda([this](const TArray<FGFGameRound>& Rounds) {
        // Process array of rounds for display
        // Calculate stats from Rounds data
    });
    
    // Fetch user game rounds (page 1)
    RoundsSystem->FetchUserGameRounds(1, OnFetchUserGameRounds);
}
```

## Updating a Game Round

You can update an existing game round:

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
            // Use UpdatedRoundResult to show final round results
        });
        
        // Update the game round
        RoundsSystem->UpdateGameRound(UpdatedRound, OnUpdateGameRound);
    });
    
    // First fetch the existing round
    RoundsSystem->FetchGameRound(RoundId, OnFetchForUpdate);
}
```

## Deleting a Game Round

You can delete a game round:

```cpp
void UMyGameMode::DeleteGameRound(int32 RoundId)
{
    UGameFuseRounds* RoundsSystem = GetGameInstance()->GetSubsystem<UGameFuseRounds>();
    
    // Create typed callback
    FGFGameRoundActionCallback OnDeleteGameRound;
    OnDeleteGameRound.BindLambda([this](bool bSuccess) {
        // Handle success or failure of round deletion
    });
    
    // Delete the game round
    RoundsSystem->DeleteGameRound(RoundId, OnDeleteGameRound);
}
```