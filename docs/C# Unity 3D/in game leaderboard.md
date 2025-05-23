Leaderboards can be easily created and managed within GameFuse from the Unity game client.
A leaderboard entry can be submitted with:

- `leaderboard_name`
- `score`
- `metadata` (optional extra attributes)

for the current signed in user.

Leaderboards can be retrieved for a specific `leaderboard_name`, which would
gather and sort the high scores for all users in the game. Leaderboards can
also be retrieved for a specific user.

## Submitting Leaderboard Entries

Use `SubmitLeaderboardEntryAsync` to add a new leaderboard entry for the current user:

!!! example
    ```csharp
    async void SubmitScore()
    {
        try
        {
            var metadata = new Dictionary<string, object>
            {
                {"deaths", 15},
                {"jewels", 12},
                {"level", "forest"}
            };
            
            var user = await GameFuseUser.CurrentUser.SubmitLeaderboardEntryAsync(
                "Game1Leaderboard", 
                1500.5, 
                metadata);
                
            Debug.Log($"Score submitted successfully for user: {user.Username}");
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Error submitting score: {ex.Message}");
        }
    }
    ```

## Getting Leaderboard Entries

### Get Global Leaderboard

Use `GetLeaderboardEntriesAsync` to retrieve leaderboard entries for all users in a specific leaderboard:

!!! example
    ```csharp
    async void GetGlobalLeaderboard()
    {
        try
        {
            int gameId = GameFuseSettings.GameId; // Your game ID from settings
            var response = await GameFuseUser.CurrentUser.GetLeaderboardEntriesAsync(
                gameId, 
                "Game1Leaderboard", 
                10); // Get top 10 entries
                
            Debug.Log($"Retrieved {response.LeaderboardEntries.Count} entries");
            
            foreach (var entry in response.LeaderboardEntries)
            {
                Debug.Log($"{entry.Username}: {entry.Score} (Rank: {entry.Rank})");
                
                // Access metadata if available
                if (entry.Metadata != null)
                {
                    foreach (var kvp in entry.Metadata)
                    {
                        Debug.Log($"  {kvp.Key}: {kvp.Value}");
                    }
                }
            }
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Error getting leaderboard: {ex.Message}");
        }
    }
    ```

### Get Current User's Leaderboard Entries

Use `GetCurrentUserLeaderboardEntriesAsync` to retrieve the current user's leaderboard entries:

!!! example
    ```csharp
    async void GetMyLeaderboardEntries()
    {
        try
        {
            // Get all leaderboard entries for current user
            var response = await GameFuseUser.CurrentUser.GetCurrentUserLeaderboardEntriesAsync(50);
            
            Debug.Log($"Current user has {response.LeaderboardEntries.Count} leaderboard entries");
            
            foreach (var entry in response.LeaderboardEntries)
            {
                Debug.Log($"Leaderboard: {entry.LeaderboardName}, Score: {entry.Score}");
            }
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Error getting user entries: {ex.Message}");
        }
    }
    
    async void GetMyEntriesForSpecificLeaderboard()
    {
        try
        {
            // Get entries for specific leaderboard, one per user
            var response = await GameFuseUser.CurrentUser.GetCurrentUserLeaderboardEntriesAsync(
                10, 
                "Game1Leaderboard", 
                true); // onePerUser = true
                
            Debug.Log($"Found {response.LeaderboardEntries.Count} entries for Game1Leaderboard");
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Error getting specific leaderboard entries: {ex.Message}");
        }
    }
    ```

### Get Another User's Leaderboard Entries

Use `GetUserLeaderboardEntriesAsync` to retrieve leaderboard entries for a specific user:

!!! example
    ```csharp
    async void GetOtherUserEntries()
    {
        try
        {
            int otherUserId = 123;
            var response = await GameFuseUser.CurrentUser.GetUserLeaderboardEntriesAsync(
                otherUserId, 
                20, 
                "Game1Leaderboard");
                
            Debug.Log($"User {otherUserId} has {response.LeaderboardEntries.Count} entries");
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Error getting other user's entries: {ex.Message}");
        }
    }
    ```

## Clearing Leaderboard Entries

Use `ClearLeaderboardEntriesAsync` to remove all entries for the current user from a specific leaderboard:

!!! example
    ```csharp
    async void ClearMyEntries()
    {
        try
        {
            var user = await GameFuseUser.CurrentUser.ClearLeaderboardEntriesAsync("Game1Leaderboard");
            Debug.Log("Successfully cleared all entries from Game1Leaderboard");
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Error clearing entries: {ex.Message}");
        }
    }
    ```

## Complete Example

Here's a comprehensive example showing leaderboard operations:

!!! example
    ```csharp
    using System.Collections.Generic;
    using UnityEngine;
    using GameFuse;
    using GameFuse.Config;
    
    public class LeaderboardExample : MonoBehaviour
    {
        async void Start()
        {
            // Ensure user is authenticated first
            if (GameFuseUser.CurrentUser == null)
            {
                Debug.LogError("User must be authenticated first");
                return;
            }
            
            await DemonstrateLeaderboardOperations();
        }
        
        async System.Threading.Tasks.Task DemonstrateLeaderboardOperations()
        {
            try
            {
                string leaderboardName = "HighScores";
                
                // 1. Submit a score with metadata
                var metadata = new Dictionary<string, object>
                {
                    {"level", "boss_fight"},
                    {"time_played", 300},
                    {"difficulty", "hard"}
                };
                
                await GameFuseUser.CurrentUser.SubmitLeaderboardEntryAsync(
                    leaderboardName, 
                    2500.75, 
                    metadata);
                Debug.Log("Score submitted successfully");
                
                // 2. Get global leaderboard (top 10)
                var globalResponse = await GameFuseUser.CurrentUser.GetLeaderboardEntriesAsync(
                    GameFuseSettings.GameId, 
                    leaderboardName, 
                    10);
                    
                Debug.Log($"Global leaderboard has {globalResponse.LeaderboardEntries.Count} entries");
                for (int i = 0; i < globalResponse.LeaderboardEntries.Count; i++)
                {
                    var entry = globalResponse.LeaderboardEntries[i];
                    Debug.Log($"{i + 1}. {entry.Username}: {entry.Score}");
                }
                
                // 3. Get current user's entries
                var myResponse = await GameFuseUser.CurrentUser.GetCurrentUserLeaderboardEntriesAsync(
                    20, 
                    leaderboardName);
                    
                Debug.Log($"I have {myResponse.LeaderboardEntries.Count} entries in {leaderboardName}");
                
                // 4. Optional: Clear entries if needed
                // await GameFuseUser.CurrentUser.ClearLeaderboardEntriesAsync(leaderboardName);
                // Debug.Log("Entries cleared");
            }
            catch (System.Exception ex)
            {
                Debug.LogError($"Error in leaderboard operations: {ex.Message}");
            }
        }
    }
    ```

## Method Reference

### `SubmitLeaderboardEntryAsync`
Submits a new leaderboard entry for the current user.

**Parameters:**
- `leaderboardName` (string): Name of the leaderboard within the game
- `score` (double): Score for the leaderboard entry
- `metadata` (Dictionary<string, object>, optional): Optional metadata for the entry
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<User>` - The user details after submission

### `ClearLeaderboardEntriesAsync`
Clears all leaderboard entries for the current user from a specific leaderboard.

**Parameters:**
- `leaderboardName` (string): Name of the leaderboard to clear entries from
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<User>` - The user details after clearing

### `GetLeaderboardEntriesAsync`
Gets leaderboard entries for a specific leaderboard (global leaderboard).

**Parameters:**
- `gameId` (int): The ID of the game
- `leaderboardName` (string): Name of the leaderboard within the game
- `limit` (int): Limit the number of results (must be >= 1)
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<LeaderboardEntriesResponse>` - List of leaderboard entries

### `GetCurrentUserLeaderboardEntriesAsync`
Gets all leaderboard entries for the current user.

**Parameters:**
- `limit` (int): Limit the number of results (must be >= 1)
- `leaderboardName` (string, optional): Name of specific leaderboard. If null, returns all entries
- `onePerUser` (bool?, optional): If true, get only one result per player on the leaderboard
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<LeaderboardEntriesResponse>` - List of leaderboard entries

### `GetUserLeaderboardEntriesAsync`
Gets leaderboard entries for a specific user.

**Parameters:**
- `userId` (int): The ID of the user whose entries to retrieve
- `limit` (int): Limit the number of results (must be >= 1)
- `leaderboardName` (string, optional): Name of specific leaderboard. If null, returns all entries
- `onePerUser` (bool?, optional): If true, get only one result per player on the leaderboard
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<LeaderboardEntriesResponse>` - List of leaderboard entries

## Error Handling

All methods are async and may throw exceptions. Always wrap calls in try-catch blocks or handle exceptions appropriately. Common error scenarios include:

- Network connectivity issues
- Invalid authentication
- Invalid leaderboard names
- Invalid user IDs
- Server errors

## Function return values

### HTTP Status Codes

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK |
| `400`            | Invalid parameters (e.g., invalid metadata, negative limit) |
| `401`            | Unauthorized - user not authenticated |
| `404`            | No entries found for the specified leaderboard name |
| `500`            | Unknown server error |