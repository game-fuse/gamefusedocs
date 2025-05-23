# Game Rounds System

GameFuse provides a robust game rounds system that allows you to track gameplay sessions and record player performance data. The game rounds functionality is accessible through the authenticated `GameFuseUser.CurrentUser` instance.

## Getting Started

All game rounds methods require user authentication. Ensure you have a signed-in user before calling any game rounds methods:

!!! example
    ```csharp
    if (GameFuseUser.CurrentUser == null)
    {
        Debug.LogError("User must be authenticated first");
        return;
    }
    ```

## Creating Game Rounds

### Create Single Player Game Round

Use `CreateGameRoundAsync` to create a new single-player game round:

!!! example
    ```csharp
    async void CreateSinglePlayerRound()
    {
        try
        {
            var metadata = new Dictionary<string, object>
            {
                {"difficulty", "hard"},
                {"level", "forest"},
                {"weapons_used", 3}
            };
            
            var gameRound = await GameFuseUser.CurrentUser.CreateGameRoundAsync(
                "battle_royale",
                DateTime.UtcNow.ToString("o"), // startTime
                null, // endTime (set when game ends)
                1500, // score
                1, // place
                metadata);
                
            Debug.Log($"Game round created! ID: {gameRound.Id}");
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Failed to create game round: {ex.Message}");
        }
    }
    ```

### Create Multiplayer Game Round

Use `CreateMultiplayerGameRoundAsync` to create or join a multiplayer game round:

!!! example
    ```csharp
    async void CreateMultiplayerRound()
    {
        try
        {
            // Create a new multiplayer game round
            var gameRound = await GameFuseUser.CurrentUser.CreateMultiplayerGameRoundAsync(
                "team_deathmatch",
                DateTime.UtcNow.ToString("o"),
                null, // endTime
                2500, // score
                2, // place
                new Dictionary<string, object> { {"team", "blue"} });
                
            Debug.Log($"Multiplayer game round created! ID: {gameRound.Id}");
            Debug.Log($"Multiplayer Round ID: {gameRound.MultiplayerGameRoundId}");
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Failed to create multiplayer game round: {ex.Message}");
        }
    }
    
    async void JoinExistingMultiplayerRound(int existingMultiplayerRoundId)
    {
        try
        {
            // Join an existing multiplayer game round
            var gameRound = await GameFuseUser.CurrentUser.CreateMultiplayerGameRoundAsync(
                "team_deathmatch",
                DateTime.UtcNow.ToString("o"),
                null,
                1800,
                3,
                new Dictionary<string, object> { {"team", "red"} },
                existingMultiplayerRoundId); // Join existing round
                
            Debug.Log($"Joined multiplayer round {existingMultiplayerRoundId}");
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Failed to join multiplayer game round: {ex.Message}");
        }
    }
    ```

## Retrieving Game Rounds

### Get Specific Game Round

Use `GetGameRoundAsync` to retrieve details for a specific game round:

!!! example
    ```csharp
    async void LoadGameRoundDetails(int gameRoundId)
    {
        try
        {
            var gameRound = await GameFuseUser.CurrentUser.GetGameRoundAsync(gameRoundId);
            
            Debug.Log($"Game Round ID: {gameRound.Id}");
            Debug.Log($"Game Type: {gameRound.GameType}");
            Debug.Log($"Score: {gameRound.Score}, Place: {gameRound.Place}");
            Debug.Log($"Start: {gameRound.StartTime}, End: {gameRound.EndTime}");
            Debug.Log($"Is Multiplayer: {gameRound.IsMultiplayer}");
            
            // Access metadata
            if (gameRound.Metadata != null)
            {
                foreach (var kvp in gameRound.Metadata)
                {
                    Debug.Log($"Metadata - {kvp.Key}: {kvp.Value}");
                }
            }
            
            // For multiplayer rounds, show rankings
            if (gameRound.IsMultiplayer && gameRound.Rankings != null)
            {
                Debug.Log("Rankings:");
                foreach (var ranking in gameRound.Rankings)
                {
                    Debug.Log($"  {ranking.Place}. {ranking.Username}: {ranking.Score}");
                }
            }
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Failed to get game round: {ex.Message}");
        }
    }
    ```

### Get Current User's Game Rounds

Use `GetCurrentUserGameRoundsAsync` to retrieve all game rounds for the current user:

!!! example
    ```csharp
    async void LoadMyGameRounds()
    {
        try
        {
            var gameRounds = await GameFuseUser.CurrentUser.GetCurrentUserGameRoundsAsync(
                page: 1, 
                perPage: 50);
                
            Debug.Log($"Found {gameRounds.Count} game rounds");
            
            foreach (var round in gameRounds)
            {
                Debug.Log($"Round {round.Id}: {round.GameType} - Score: {round.Score}");
            }
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Failed to get user game rounds: {ex.Message}");
        }
    }
    ```

### Get Another User's Game Rounds

Use `GetGameRoundsForUserAsync` to retrieve game rounds for a specific user:

!!! example
    ```csharp
    async void LoadOtherUserGameRounds(int userId)
    {
        try
        {
            var gameRounds = await GameFuseUser.CurrentUser.GetGameRoundsForUserAsync(
                userId, 
                page: 1, 
                perPage: 25);
                
            Debug.Log($"User {userId} has {gameRounds.Count} game rounds");
            
            foreach (var round in gameRounds)
            {
                Debug.Log($"Round: {round.GameType} - Score: {round.Score}, Place: {round.Place}");
            }
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Failed to get user's game rounds: {ex.Message}");
        }
    }
    ```

## Updating Game Rounds

Use `UpdateGameRoundAsync` to update an existing game round:

!!! example
    ```csharp
    async void UpdateGameRound(int gameRoundId)
    {
        try
        {
            var updatedMetadata = new Dictionary<string, object>
            {
                {"final_boss_defeated", true},
                {"powerups_collected", 8}
            };
            
            var updatedRound = await GameFuseUser.CurrentUser.UpdateGameRoundAsync(
                gameRoundId,
                null, // startTime (don't change)
                DateTime.UtcNow.ToString("o"), // endTime
                3000, // new score
                1, // new place
                "boss_battle", // new game type
                updatedMetadata);
                
            Debug.Log($"Game round updated! New score: {updatedRound.Score}");
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Failed to update game round: {ex.Message}");
        }
    }
    ```

## Deleting Game Rounds

Use `DeleteGameRoundAsync` to delete a game round:

!!! example
    ```csharp
    async void DeleteGameRound(int gameRoundId)
    {
        try
        {
            var response = await GameFuseUser.CurrentUser.DeleteGameRoundAsync(gameRoundId);
            Debug.Log($"Game round deleted: {response.Message}");
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Failed to delete game round: {ex.Message}");
        }
    }
    ```

## Leaderboard Features

### Get Game Leaderboard

Use `GetLeaderboardAsync` to retrieve the game's leaderboard:

!!! example
    ```csharp
    async void LoadLeaderboard()
    {
        try
        {
            var leaderboard = await GameFuseUser.CurrentUser.GetLeaderboardAsync(limit: 10);
            
            Debug.Log($"Top {leaderboard.Count} players:");
            foreach (var entry in leaderboard)
            {
                Debug.Log($"{entry.Rank}. {entry.Username}: {entry.Score}");
            }
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Failed to get leaderboard: {ex.Message}");
        }
    }
    ```

### Get User's Rank

Use `GetUserRankAsync` to get the current user's leaderboard position:

!!! example
    ```csharp
    async void LoadMyRank()
    {
        try
        {
            var myRank = await GameFuseUser.CurrentUser.GetUserRankAsync();
            Debug.Log($"My rank: {myRank.Rank}, Score: {myRank.Score}");
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Failed to get user rank: {ex.Message}");
        }
    }
    ```

## Complete Example

Here's a comprehensive example showing how to implement a game rounds management system:

!!! example
    ```csharp
    using System.Collections.Generic;
    using UnityEngine;
    using UnityEngine.UI;
    using GameFuse;
    using GameFuse.Models.Shared;
    using System;
    
    public class GameRoundsManager : MonoBehaviour
    {
        [Header("UI References")]
        public Transform gameRoundsListParent;
        public GameObject gameRoundItemPrefab;
        public Button startGameButton;
        public Button endGameButton;
        public Text currentScoreText;
        public Text leaderboardText;
        
        [Header("Game Settings")]
        public string currentGameType = "survival";
        public bool isMultiplayerMode = false;
        
        private GameRound currentGameRound;
        private bool gameInProgress = false;
        private int currentScore = 0;
        private DateTime gameStartTime;
        
        void Start()
        {
            startGameButton.onClick.AddListener(OnStartGameClicked);
            endGameButton.onClick.AddListener(OnEndGameClicked);
            
            LoadGameHistory();
            LoadLeaderboard();
        }
        
        async void OnStartGameClicked()
        {
            if (gameInProgress)
            {
                Debug.LogWarning("Game already in progress");
                return;
            }
            
            await StartNewGame();
        }
        
        async void OnEndGameClicked()
        {
            if (!gameInProgress)
            {
                Debug.LogWarning("No game in progress");
                return;
            }
            
            await EndCurrentGame();
        }
        
        public async System.Threading.Tasks.Task StartNewGame()
        {
            try
            {
                gameStartTime = DateTime.UtcNow;
                currentScore = 0;
                
                var metadata = new Dictionary<string, object>
                {
                    {"difficulty", "normal"},
                    {"map", "default"}
                };
                
                if (isMultiplayerMode)
                {
                    currentGameRound = await GameFuseUser.CurrentUser.CreateMultiplayerGameRoundAsync(
                        currentGameType,
                        gameStartTime.ToString("o"),
                        null, // endTime
                        currentScore,
                        null, // place (TBD)
                        metadata);
                        
                    Debug.Log($"Started multiplayer game! Round ID: {currentGameRound.Id}");
                }
                else
                {
                    currentGameRound = await GameFuseUser.CurrentUser.CreateGameRoundAsync(
                        currentGameType,
                        gameStartTime.ToString("o"),
                        null, // endTime
                        currentScore,
                        null, // place (TBD)
                        metadata);
                        
                    Debug.Log($"Started single-player game! Round ID: {currentGameRound.Id}");
                }
                
                gameInProgress = true;
                startGameButton.interactable = false;
                endGameButton.interactable = true;
                
                // Start game loop
                InvokeRepeating(nameof(UpdateGameProgress), 1f, 1f);
            }
            catch (System.Exception ex)
            {
                Debug.LogError($"Error starting game: {ex.Message}");
            }
        }
        
        void UpdateGameProgress()
        {
            if (!gameInProgress) return;
            
            // Simulate score increase
            currentScore += UnityEngine.Random.Range(10, 50);
            currentScoreText.text = $"Score: {currentScore}";
        }
        
        public async System.Threading.Tasks.Task EndCurrentGame()
        {
            try
            {
                CancelInvoke(nameof(UpdateGameProgress));
                
                var endTime = DateTime.UtcNow;
                var finalPlace = UnityEngine.Random.Range(1, 11); // Random place 1-10
                
                var finalMetadata = new Dictionary<string, object>
                {
                    {"duration_seconds", (endTime - gameStartTime).TotalSeconds},
                    {"final_level_reached", UnityEngine.Random.Range(5, 20)}
                };
                
                var updatedRound = await GameFuseUser.CurrentUser.UpdateGameRoundAsync(
                    currentGameRound.Id,
                    null, // don't change start time
                    endTime.ToString("o"),
                    currentScore,
                    finalPlace,
                    null, // don't change game type
                    finalMetadata);
                    
                Debug.Log($"Game ended! Final score: {currentScore}, Place: {finalPlace}");
                
                gameInProgress = false;
                startGameButton.interactable = true;
                endGameButton.interactable = false;
                
                // Refresh displays
                await LoadGameHistory();
                await LoadLeaderboard();
            }
            catch (System.Exception ex)
            {
                Debug.LogError($"Error ending game: {ex.Message}");
            }
        }
        
        async System.Threading.Tasks.Task LoadGameHistory()
        {
            try
            {
                var gameRounds = await GameFuseUser.CurrentUser.GetCurrentUserGameRoundsAsync(1, 10);
                
                // Clear existing items
                foreach (Transform child in gameRoundsListParent)
                {
                    Destroy(child.gameObject);
                }
                
                // Create UI items for each game round
                foreach (var round in gameRounds)
                {
                    var roundItem = Instantiate(gameRoundItemPrefab, gameRoundsListParent);
                    var roundUI = roundItem.GetComponent<GameRoundItemUI>();
                    if (roundUI != null)
                    {
                        roundUI.Setup(round, this);
                    }
                }
            }
            catch (System.Exception ex)
            {
                Debug.LogError($"Error loading game history: {ex.Message}");
            }
        }
        
        async System.Threading.Tasks.Task LoadLeaderboard()
        {
            try
            {
                var leaderboard = await GameFuseUser.CurrentUser.GetLeaderboardAsync(10);
                var myRank = await GameFuseUser.CurrentUser.GetUserRankAsync();
                
                string leaderboardText = "Top 10 Players:\\n";
                foreach (var entry in leaderboard)
                {
                    leaderboardText += $"{entry.Rank}. {entry.Username}: {entry.Score}\\n";
                }
                
                leaderboardText += $"\\nYour Rank: {myRank.Rank} (Score: {myRank.Score})";
                this.leaderboardText.text = leaderboardText;
            }
            catch (System.Exception ex)
            {
                Debug.LogError($"Error loading leaderboard: {ex.Message}");
            }
        }
        
        public async System.Threading.Tasks.Task ViewGameRoundDetails(int gameRoundId)
        {
            try
            {
                var round = await GameFuseUser.CurrentUser.GetGameRoundAsync(gameRoundId);
                
                string details = $"Game Round {round.Id}\\n";
                details += $"Type: {round.GameType}\\n";
                details += $"Score: {round.Score}, Place: {round.Place}\\n";
                details += $"Start: {round.StartTime}\\n";
                details += $"End: {round.EndTime}\\n";
                details += $"Multiplayer: {round.IsMultiplayer}\\n";
                
                if (round.Metadata != null)
                {
                    details += "Metadata:\\n";
                    foreach (var kvp in round.Metadata)
                    {
                        details += $"  {kvp.Key}: {kvp.Value}\\n";
                    }
                }
                
                Debug.Log(details);
            }
            catch (System.Exception ex)
            {
                Debug.LogError($"Error viewing game round details: {ex.Message}");
            }
        }
        
        public async System.Threading.Tasks.Task DeleteGameRound(int gameRoundId)
        {
            try
            {
                await GameFuseUser.CurrentUser.DeleteGameRoundAsync(gameRoundId);
                Debug.Log($"Deleted game round {gameRoundId}");
                await LoadGameHistory();
            }
            catch (System.Exception ex)
            {
                Debug.LogError($"Error deleting game round: {ex.Message}");
            }
        }
    }
    
    public class GameRoundItemUI : MonoBehaviour
    {
        public Text gameTypeText;
        public Text scoreText;
        public Text dateText;
        public Button viewButton;
        public Button deleteButton;
        
        private GameRound gameRound;
        private GameRoundsManager manager;
        
        void Start()
        {
            viewButton.onClick.AddListener(() => manager.ViewGameRoundDetails(gameRound.Id));
            deleteButton.onClick.AddListener(() => manager.DeleteGameRound(gameRound.Id));
        }
        
        public void Setup(GameRound round, GameRoundsManager gameManager)
        {
            gameRound = round;
            manager = gameManager;
            
            gameTypeText.text = round.GameType ?? "Unknown";
            scoreText.text = $"Score: {round.Score}, Place: {round.Place}";
            
            if (DateTime.TryParse(round.EndTime, out DateTime endDate))
            {
                dateText.text = endDate.ToString("MM/dd/yyyy HH:mm");
            }
            else
            {
                dateText.text = "In Progress";
            }
        }
    }
    ```

## Method Reference

### `CreateGameRoundAsync`
Creates a new single-player game round.

**Parameters:**
- `gameType` (string): Type of game being played
- `startTime` (string, optional): Start time in ISO format
- `endTime` (string, optional): End time in ISO format
- `score` (int?, optional): Player's score
- `place` (int?, optional): Player's final position
- `metadata` (Dictionary<string, object>, optional): Additional game data
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<GameRound>` - The created game round

### `CreateMultiplayerGameRoundAsync`
Creates a new multiplayer game round or joins an existing one.

**Parameters:**
- `gameType` (string): Type of game being played
- `startTime` (string, optional): Start time in ISO format
- `endTime` (string, optional): End time in ISO format
- `score` (int?, optional): Player's score
- `place` (int?, optional): Player's final position
- `metadata` (Dictionary<string, object>, optional): Additional game data
- `multiplayerGameRoundId` (int?, optional): ID of existing multiplayer round to join
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<GameRound>` - The created game round

### `GetGameRoundAsync`
Retrieves a specific game round by ID.

**Parameters:**
- `gameRoundId` (int): ID of the game round to retrieve
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<GameRound>` - The game round with full details including rankings for multiplayer rounds

### `GetCurrentUserGameRoundsAsync`
Retrieves game rounds for the current user with pagination.

**Parameters:**
- `page` (int, optional): Page number (default 1)
- `perPage` (int, optional): Number of rounds per page (default and max 100)
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<IReadOnlyList<GameRound>>` - List of game rounds (bulk retrieval doesn't include rankings)

### `GetGameRoundsForUserAsync`
Retrieves game rounds for a specific user with pagination.

**Parameters:**
- `userId` (int): ID of the user whose rounds to retrieve
- `page` (int, optional): Page number (default 1)
- `perPage` (int, optional): Number of rounds per page (default and max 100)
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<IReadOnlyList<GameRound>>` - List of game rounds (bulk retrieval doesn't include rankings)

### `UpdateGameRoundAsync`
Updates an existing game round.

**Parameters:**
- `gameRoundId` (int): ID of the game round to update
- `startTime` (string, optional): New start time
- `endTime` (string, optional): New end time
- `score` (int?, optional): New score
- `place` (int?, optional): New place
- `gameType` (string, optional): New game type
- `metadata` (Dictionary<string, object>, optional): New metadata
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<GameRound>` - The updated game round

### `DeleteGameRoundAsync`
Deletes a game round.

**Parameters:**
- `gameRoundId` (int): ID of the game round to delete
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<GameRoundDeleteResponse>` - Response confirming deletion

### `GetLeaderboardAsync`
Retrieves the game's leaderboard.

**Parameters:**
- `limit` (int, optional): Maximum number of entries to return (default 100)
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<IReadOnlyList<LeaderboardEntry>>` - List of leaderboard entries

### `GetUserRankAsync`
Gets the current user's leaderboard rank.

**Parameters:**
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<LeaderboardEntry>` - The user's leaderboard entry with rank and score

## Error Handling

All game rounds methods are async and may throw exceptions. Always wrap calls in try-catch blocks:

- **Network connectivity issues** - Connection problems
- **Authentication errors** - User not signed in
- **Permission errors** - Attempting to modify rounds owned by other users
- **Invalid parameters** - Round not found, invalid data formats
- **Server errors** - Temporary GameFuse service issues

## Function return values

### HTTP Status Codes

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK - Operation completed successfully |
| `400`            | Bad Request - Invalid parameters or data format |
| `401`            | Unauthorized - User not authenticated |
| `403`            | Forbidden - Insufficient permissions |
| `404`            | Not Found - Game round not found |
| `500`            | Internal Server Error - Server error |

## Important Notes

- **Pagination**: Bulk game round retrieval methods support pagination with a maximum of 100 rounds per page
- **Rankings**: Multiplayer game round rankings are only included when fetching individual rounds via `GetGameRoundAsync`
- **Metadata**: Use metadata to store custom game-specific information as key-value pairs
- **Time Format**: Use ISO 8601 format for start and end times (`DateTime.UtcNow.ToString("o")`)
- **Multiplayer Rounds**: Players can join existing multiplayer rounds or create new ones