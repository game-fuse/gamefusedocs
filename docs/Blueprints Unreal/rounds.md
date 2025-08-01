# Rounds System

The GameFuse Rounds System allows you to track and manage game rounds in your game. This includes creating rounds, fetching round data, updating rounds, and managing round metadata.

## Getting Started with Rounds

To use the GameFuse Rounds system in Blueprints, you'll need to access the GameFuse Rounds subsystem through **Get Game Instance** → **Get Subsystem** → **GameFuse Rounds**.

## Creating a Game Round

Create a new game round to track player performance and statistics:

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/5dl5z2g-/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

## Fetching Game Rounds

### Fetch All Rounds

Get all game rounds for the current user:

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/-2v924k2/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

### Fetch Rounds by Game Type

Get rounds filtered by a specific game type:

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/-2v924k2/" width="800" height="600" frameborder="0" allowfullscreen></iframe>


## Updating a Game Round

Update an existing game round with new data:

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/ia1je9m2/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

## Deleting a Game Round

Remove a game round from the system:

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/ix988if_/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

## Function Parameters

### Create Game Round

| Parameter | Type | Description |
|-----------|------|-------------|
| `Score` | `Integer` | Player's score for this round |
| `Game Type` | `String` | Type/category of the game round |
| `Start Time` | `String` | When the round started (ISO format, optional) |
| `End Time` | `String` | When the round ended (ISO format, optional) |
| `Metadata` | `String` | Additional data in JSON format (optional) |

### Update Game Round

| Parameter | Type | Description |
|-----------|------|-------------|
| `Round ID` | `Integer` | Unique identifier of the round to update |
| `Score` | `Integer` | Updated score (optional) |
| `Game Type` | `String` | Updated game type (optional) |
| `Metadata` | `String` | Updated metadata in JSON format (optional) |

### Fetch/Delete Game Round

| Parameter | Type | Description |
|-----------|------|-------------|
| `Round ID` | `Integer` | Unique identifier of the round (for deletion) |
| `Game Type` | `String` | Filter by game type (for filtered fetch) |

## Function Return Values

### Create/Update Game Round

| HTTP Status Code | Description |
|------------------|-------------|
| `200` | OK - Round created/updated successfully |
| `400` | Bad request - Invalid parameters |
| `401` | Unauthorized - User not signed in |
| `500` | Unknown server error |

### Fetch Game Rounds

| HTTP Status Code | Description |
|------------------|-------------|
| `200` | OK - Rounds fetched successfully |
| `401` | Unauthorized - User not signed in |
| `500` | Unknown server error |

### Delete Game Round

| HTTP Status Code | Description |
|------------------|-------------|
| `200` | OK - Round deleted successfully |
| `400` | Bad request - Invalid round ID |
| `401` | Unauthorized - User not signed in |
| `404` | Round not found |
| `500` | Unknown server error |

## Data Structures

### Game Round Struct

| Property | Type | Description |
|----------|------|-------------|
| `ID` | `Integer` | Unique round identifier |
| `User ID` | `Integer` | ID of the user who played this round |
| `Score` | `Integer` | Player's score for this round |
| `Game Type` | `String` | Type/category of the game round |
| `Start Time` | `String` | When the round started |
| `End Time` | `String` | When the round ended |
| `Duration` | `Integer` | Round duration in seconds |
| `Metadata` | `String` | Additional data in JSON format |
| `Created At` | `String` | When the round record was created |
| `Updated At` | `String` | When the round record was last updated |

## Next Steps

- [In Game Leaderboard](in%20game%20leaderboard.md) - Use round scores for leaderboards
- [Using Credits](using%20credits.md) - Award credits based on performance
- [Custom User Data](custom%20user%20data.md) - Store additional player statistics
- [Friends](friends.md) - Compare round performance with friends 