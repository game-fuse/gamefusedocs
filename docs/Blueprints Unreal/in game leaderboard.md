# In Game Leaderboard

GameFuse provides a comprehensive leaderboard system that allows you to track and display player scores, achievements, and progress across different categories.

## Overview

The leaderboard system supports multiple named leaderboards, allowing you to categorize different types of achievements (e.g., "High Score", "Speed Run", "Daily Challenge"). Each entry can include additional attributes for rich metadata.

## Adding Leaderboard Entries

### Basic Score Entry

You can add a simple score entry to a leaderboard:

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/4krvppva/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

### Score Entry with Attributes

For more detailed leaderboard entries, you can include additional metadata:

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/4krvppva/" width="800" height="600" frameborder="0" allowfullscreen></iframe>


## Fetching Leaderboard Entries

### Fetch All Entries for a Leaderboard

Get all high scores for a specific leaderboard:

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/ysmr11jw/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

### Fetch User-Specific Entries

Get all entries for the current user across a specific leaderboard:

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/ysmr11jw/" width="800" height="600" frameborder="0" allowfullscreen></iframe>


## Clearing Leaderboard Entries

You can clear all entries for the current user on a specific leaderboard:

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/ctt8-0po/" width="800" height="600" frameborder="0" allowfullscreen></iframe>


## Function Parameters

### Add Leaderboard Entry

| Parameter | Type | Description |
|-----------|------|-------------|
| `Leaderboard Name` | `String` | The name/category of the leaderboard |
| `Score` | `Integer` | The score value to add |

### Add Leaderboard Entry With Attributes

| Parameter | Type | Description |
|-----------|------|-------------|
| `Leaderboard Name` | `String` | The name/category of the leaderboard |
| `Score` | `Integer` | The score value to add |
| `Extra Attributes` | `String` | Additional metadata in JSON format |

### Fetch/Clear Leaderboard Entries

| Parameter | Type | Description |
|-----------|------|-------------|
| `Leaderboard Name` | `String` | The name/category of the leaderboard |

## Function Return Values

### Add Leaderboard Entry

| HTTP Status Code | Description |
|------------------|-------------|
| `200` | OK - Entry added successfully |
| `400` | Bad request - Invalid parameters |
| `401` | Unauthorized - User not signed in |
| `500` | Unknown server error |

### Fetch Leaderboard Entries

| HTTP Status Code | Description |
|------------------|-------------|
| `200` | OK - Entries fetched successfully |
| `400` | Bad request - Invalid leaderboard name |
| `401` | Unauthorized - User not signed in |
| `404` | Leaderboard not found |
| `500` | Unknown server error |

### Clear Leaderboard Entries

| HTTP Status Code | Description |
|------------------|-------------|
| `200` | OK - Entries cleared successfully |
| `400` | Bad request - Invalid leaderboard name |
| `401` | Unauthorized - User not signed in |
| `500` | Unknown server error |

## Data Structures

### Leaderboard Entry Struct

| Property | Type | Description |
|----------|------|-------------|
| `ID` | `Integer` | Unique identifier for the entry |
| `Score` | `Integer` | The score value |
| `Username` | `String` | Username of the player who achieved this score |
| `User ID` | `Integer` | Unique ID of the player |
| `Leaderboard Name` | `String` | Name of the leaderboard category |
| `Extra Attributes` | `String` | Additional metadata in JSON format |
| `Created At` | `String` | Timestamp when the entry was created |


## Next Steps

- [Friends](friends.md) - Show friends' scores on leaderboards
- [Using Credits](using%20credits.md) - Reward players with credits for achievements
- [Custom User Data](custom%20user%20data.md) - Store additional player statistics
- [Groups](groups.md) - Create group-based leaderboards
