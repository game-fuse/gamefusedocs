# Game Rounds System

GameFuse provides a robust game rounds system that allows you to track gameplay sessions and record player performance data. The `GameFuseUser.GameRounds` partial class contains methods to create, retrieve, update, and delete game rounds within your game.

## Game Rounds Methods

All game rounds methods are accessible through the `GameFuseUser` instance after a successful login.

### CreateGameRoundAsync()

Creates a new basic game round for the current user.

**Parameters:**
- None

**Returns:**
- `Task<GameRoundObject>`: Response containing the created game round

**Example:**
```csharp
try
{
    // Get the current authenticated user
    GameFuseUser currentUser = GameFuseUser.CurrentUser;
    
    // Create a basic game round
    GameRoundObject createdRound = await currentUser.CreateGameRoundAsync();
    
    // Access the game round ID
    int roundId = createdRound.Id;
    Debug.Log($"Game round created successfully! Round ID: {roundId}");
}
catch (ApiException ex)
{
    Debug.LogError($"Failed to create game round: {ex.Message}");
}
```

### CreateGameRoundAsync(GameRoundObject)

Creates a new game round with detailed information for the current user.

**Parameters:**
- `gameRound` (GameRoundObject): Game round details including:
  - `GameType` (string): Type of game being played (optional)
  - `Multiplayer` (bool): Whether this is a multiplayer game round
  - `Score` (double): Player's score in the game round
  - `Place` (int): Player's final position/ranking
  - `Metadata` (Dictionary<string, string>): Additional custom data (optional)

**Returns:**
- `Task<GameRoundObject>`: Response containing the created game round

**Example:**
```csharp
try
{
    // Get the current authenticated user
    GameFuseUser currentUser = GameFuseUser.CurrentUser;
    
    // Create game round with details
    var gameRound = new GameRoundObject
    {
        GameType = "battle_royale",
        Multiplayer = true,
        Score = 1500,
        Place = 1,
        Metadata = new Dictionary<string, string>
        {
            { "difficulty", "expert" },
            { "map", "volcano" },
            { "weapons_used", "5" }
        }
    };
    
    GameRoundObject createdRound = await currentUser.CreateGameRoundAsync(gameRound);
    Debug.Log($"Detailed game round created with ID: {createdRound.Id}");
}
catch (ApiException ex)
{
    Debug.LogError($"Failed to create detailed game round: {ex.Message}");
}
```

### UpdateGameRoundAsync

Updates an existing game round owned by the current user.

**Parameters:**
- `gameRoundId` (int): ID of the game round to update
- `gameRound` (GameRoundObject): Game round with updated values

**Returns:**
- `Task<GameRoundObject>`: Response containing the updated game round

**Example:**
```csharp
try
{
    // Get the current authenticated user
    GameFuseUser currentUser = GameFuseUser.CurrentUser;
    
    // Create update data
    var updatedRound = new GameRoundObject
    {
        Score = 2000,
        Place = 2,
        EndTime = DateTime.UtcNow.ToString("o"),
        Metadata = new Dictionary<string, string>
        {
            { "powerups_used", "3" }
        }
    };
    
    // Update an existing game round
    GameRoundObject result = await currentUser.UpdateGameRoundAsync(123, updatedRound);
    
    Debug.Log($"Game round updated! New score: {result.Score}, Place: {result.Place}");
}
catch (ApiException ex)
{
    Debug.LogError($"Failed to update game round: {ex.Message}");
}
```

### GetGameRoundAsync

Retrieves a specific game round by ID.

**Parameters:**
- `gameRoundId` (int): ID of the game round to retrieve

**Returns:**
- `Task<GameRoundObject>`: Response containing the game round details

**Example:**
```csharp
try
{
    // Get the current authenticated user
    GameFuseUser currentUser = GameFuseUser.CurrentUser;
    
    // Get details for a specific game round
    GameRoundObject gameRound = await currentUser.GetGameRoundAsync(123);
    
    // Display game round information
    Debug.Log($"Game Type: {gameRound.GameType}, Score: {gameRound.Score}");
    
    // For multiplayer games, show rankings
    if (gameRound.Multiplayer && gameRound.Rankings != null)
    {
        foreach (var ranking in gameRound.Rankings)
        {
            Debug.Log($"Player: {ranking.User.Username}, Place: {ranking.Place}, Score: {ranking.Score}");
        }
    }
    
    // Access metadata
    if (gameRound.Metadata != null)
    {
        foreach (var item in gameRound.Metadata)
        {
            Debug.Log($"Metadata - {item.Key}: {item.Value}");
        }
    }
}
catch (ApiException ex)
{
    Debug.LogError($"Failed to get game round details: {ex.Message}");
}
```

### GetMyGameRoundsAsync

Retrieves all game rounds for the current user.

**Parameters:**
- None

**Returns:**
- `Task<GameRoundsResponse>`: Response containing an array of game rounds

**Example:**
```csharp
try
{
    // Get the current authenticated user
    GameFuseUser currentUser = GameFuseUser.CurrentUser;
    
    // Get all game rounds for the current user
    GameRoundsResponse response = await currentUser.GetMyGameRoundsAsync();
    
    // Display the rounds
    Debug.Log($"Found {response.GameRounds.Length} game rounds");
    
    foreach (GameRoundObject round in response.GameRounds)
    {
        Debug.Log($"Round ID: {round.Id}, Type: {round.GameType}, Score: {round.Score}");
    }
}
catch (ApiException ex)
{
    Debug.LogError($"Failed to get user game rounds: {ex.Message}");
}
```

### GetUserGameRoundsAsync

Retrieves all game rounds for a specific user.

**Parameters:**
- `userId` (int): ID of the user whose game rounds to retrieve

**Returns:**
- `Task<GameRoundsResponse>`: Response containing an array of game rounds

**Example:**
```csharp
try
{
    // Get the current authenticated user
    GameFuseUser currentUser = GameFuseUser.CurrentUser;
    
    // Get game rounds for another user
    int friendUserId = 456;
    GameRoundsResponse response = await currentUser.GetUserGameRoundsAsync(friendUserId);
    
    // Display the rounds
    Debug.Log($"Found {response.GameRounds.Length} game rounds for user {friendUserId}");
    
    foreach (GameRoundObject round in response.GameRounds)
    {
        Debug.Log($"Round ID: {round.Id}, Type: {round.GameType}, Score: {round.Score}");
    }
}
catch (ApiException ex)
{
    Debug.LogError($"Failed to get user game rounds: {ex.Message}");
}
```

### DeleteGameRoundAsync

Deletes a specific game round owned by the current user.

**Parameters:**
- `gameRoundId` (int): ID of the game round to delete

**Returns:**
- `Task<MessageResponse>`: Response confirming the deletion

**Example:**
```csharp
try
{
    // Get the current authenticated user
    GameFuseUser currentUser = GameFuseUser.CurrentUser;
    
    // Delete a game round
    MessageResponse response = await currentUser.DeleteGameRoundAsync(123);
    
    Debug.Log($"Game round deleted. Response: {response.Message}");
}
catch (ApiException ex)
{
    Debug.LogError($"Failed to delete game round: {ex.Message}");
}
```

## GameRoundObject Properties

The `GameRoundObject` class contains the following properties:

| Property | Type | Description |
|----------|------|-------------|
| `Id` | int | The ID of the game round |
| `GameId` | int | The ID of the game |
| `GameUserId` | int | The ID of the user to whom the game round belongs |
| `MultiplayerGameRoundId` | int? | ID of the associated multiplayer game round (if applicable) |
| `GameType` | string | Type of game being played |
| `Place` | int | The place the user finished in (1st, 2nd, etc.) |
| `Score` | double | The score achieved in the game round |
| `StartTime` | string | Start time of the game round |
| `EndTime` | string | End time of the game round |
| `Metadata` | Dictionary<string, string> | Additional metadata related to the game round |
| `CreatedAt` | string | When the game round was created |
| `UpdatedAt` | string | When the game round was last updated |
| `Rankings` | RankingsObject[] | For multiplayer games, contains the rankings of all participants |
| `Multiplayer` | bool | If true, indicates this is a multiplayer game round |

## Common Usage Pattern

Here's an example of how to implement a basic game rounds manager in your game:

```csharp
using UnityEngine;
using GameFuseCSharp;
using System.Collections.Generic;
using UnityEngine.UI;
using System;

public class GameRoundsManager : MonoBehaviour
{
    // UI elements for game rounds list
    [SerializeField] private Transform gameRoundsListContainer;
    [SerializeField] private GameObject gameRoundItemPrefab;

    // Track the current game round
    private GameRoundObject currentGameRound;
    private bool gameInProgress = false;

    // Start a new game round
    public async void StartNewGameRound(string gameType, bool isMultiplayer)
    {
        try
        {
            GameFuseUser currentUser = GameFuseUser.CurrentUser;
            if (currentUser == null)
            {
                Debug.LogError("User not logged in");
                return;
            }

            // Create initial game round
            var gameRound = new GameRoundObject
            {
                GameType = gameType,
                Multiplayer = isMultiplayer,
                StartTime = System.DateTime.UtcNow.ToString("o"),
                Score = 0,
                Place = 0  // Not yet determined
            };

            currentGameRound = await currentUser.CreateGameRoundAsync(gameRound);
            gameInProgress = true;

            Debug.Log($"Started new game round ID: {currentGameRound.Id}");
        }
        catch (ApiException ex)
        {
            Debug.LogError($"Error creating game round: {ex.Message}");
        }
    }

    // Update the score during gameplay
    public void UpdateScore(double newScore)
    {
        if (!gameInProgress || currentGameRound == null)
        {
            Debug.LogWarning("No game round in progress");
            return;
        }

        // Update local score
        currentGameRound.Score = newScore;
    }

    // End the current game round
    public async void EndGameRound(int finalPlace, Dictionary<string, string> gameMetadata)
    {
        if (!gameInProgress || currentGameRound == null)
        {
            Debug.LogWarning("No game round in progress");
            return;
        }

        try
        {
            GameFuseUser currentUser = GameFuseUser.CurrentUser;
            if (currentUser == null)
            {
                Debug.LogError("User not logged in");
                return;
            }

            // Update the game round with final data
            currentGameRound.Place = finalPlace;
            currentGameRound.EndTime = System.DateTime.UtcNow.ToString("o");
            currentGameRound.Metadata = gameMetadata;

            // Send the update to the server
            await currentUser.UpdateGameRoundAsync(currentGameRound.Id, currentGameRound);

            gameInProgress = false;
            Debug.Log($"Game round {currentGameRound.Id} completed with score: {currentGameRound.Score}");
        }
        catch (ApiException ex)
        {
            Debug.LogError($"Error ending game round: {ex.Message}");
        }
    }

    // Load recent game rounds
    public async void LoadRecentGameRounds()
    {
        try
        {
            GameFuseUser currentUser = GameFuseUser.CurrentUser;
            if (currentUser == null)
            {
                Debug.LogError("User not logged in");
                return;
            }

            // Clear existing items
            foreach (Transform child in gameRoundsListContainer)
            {
                Destroy(child.gameObject);
            }

            // Get all game rounds for the current user
            GameRoundsResponse response = await currentUser.GetMyGameRoundsAsync();

            // Create UI items for each game round
            foreach (GameRoundObject round in response.GameRounds)
            {
                GameObject roundItem = Instantiate(gameRoundItemPrefab, gameRoundsListContainer);
                GameRoundItemUI roundUI = roundItem.GetComponent<GameRoundItemUI>();
                roundUI.SetupGameRound(round);
            }

            Debug.Log($"Loaded {response.GameRounds.Length} game rounds");
        }
        catch (ApiException ex)
        {
            Debug.LogError($"Error loading game rounds: {ex.Message}");
        }
    }

    // View details for a specific game round
    public async void ViewGameRoundDetails(int gameRoundId)
    {
        try
        {
            GameFuseUser currentUser = GameFuseUser.CurrentUser;
            if (currentUser == null)
            {
                Debug.LogError("User not logged in");
                return;
            }

            // Get the game round details
            GameRoundObject round = await currentUser.GetGameRoundAsync(gameRoundId);

            // Here you would update your UI with the detailed information
            Debug.Log($"Viewing details for game round {round.Id}:");
            Debug.Log($"Game Type: {round.GameType}");
            Debug.Log($"Score: {round.Score}, Place: {round.Place}");
            Debug.Log($"Started: {round.StartTime}, Ended: {round.EndTime}");

            if (round.Metadata != null)
            {
                Debug.Log("Metadata:");
                foreach (var entry in round.Metadata)
                {
                    Debug.Log($"  {entry.Key}: {entry.Value}");
                }
            }
        }
        catch (ApiException ex)
        {
            Debug.LogError($"Error viewing game round: {ex.Message}");
        }
    }

    // Delete a game round
    public async void DeleteGameRound(int gameRoundId)
    {
        try
        {
            GameFuseUser currentUser = GameFuseUser.CurrentUser;
            if (currentUser == null)
            {
                Debug.LogError("User not logged in");
                return;
            }

            // Delete the game round
            await currentUser.DeleteGameRoundAsync(gameRoundId);

            // Refresh the list
            LoadRecentGameRounds();

            Debug.Log($"Deleted game round {gameRoundId}");
        }
        catch (ApiException ex)
        {
            Debug.LogError($"Error deleting game round: {ex.Message}");
        }
    }
}

// UI component for displaying game round items in a list
public class GameRoundItemUI : MonoBehaviour
{
    [SerializeField] private Text gameTypeText;
    [SerializeField] private Text scoreText;
    [SerializeField] private Text dateText;
    [SerializeField] private Button viewDetailsButton;
    [SerializeField] private Button deleteButton;

    private int gameRoundId;
    private GameRoundsManager gameRoundsManager;

    private void Start()
    {
        gameRoundsManager = FindObjectOfType<GameRoundsManager>();

        if (viewDetailsButton != null)
        {
            viewDetailsButton.onClick.AddListener(OnViewDetailsClicked);
        }

        if (deleteButton != null)
        {
            deleteButton.onClick.AddListener(OnDeleteClicked);
        }
    }

    public void SetupGameRound(GameRoundObject round)
    {
        gameRoundId = round.Id;

        if (gameTypeText != null)
        {
            gameTypeText.text = string.IsNullOrEmpty(round.GameType) ? "Unknown" : round.GameType;
        }

        if (scoreText != null)
        {
            scoreText.text = $"Score: {round.Score}, Place: {round.Place}";
        }

        if (dateText != null)
        {
            string dateDisplay = "Unknown date";
            if (!string.IsNullOrEmpty(round.EndTime))
            {
                if (DateTime.TryParse(round.EndTime, out DateTime endDate))
                {
                    dateDisplay = endDate.ToString("MM/dd/yyyy HH:mm");
                }
            }
            dateText.text = dateDisplay;
        }
    }

    private void OnViewDetailsClicked()
    {
        if (gameRoundsManager != null)
        {
            gameRoundsManager.ViewGameRoundDetails(gameRoundId);
        }
    }

    private void OnDeleteClicked()
    {
        if (gameRoundsManager != null)
        {
            gameRoundsManager.DeleteGameRound(gameRoundId);
        }
    }
}
```

## Error Handling

All game rounds methods throw an `ApiException` when a request fails. Always wrap your API calls in try-catch blocks to handle errors gracefully.

Common errors include:
- Insufficient permissions (trying to update or delete a game round you don't own)
- Game round not found
- Invalid data formats
- Network connectivity issues