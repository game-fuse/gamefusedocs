# Custom User Data

Custom user data allows you to store key-value pairs for each user, providing flexible data storage for game-specific information beyond the standard user properties.

## Overview

Custom user data (also known as user attributes) is a simple way to save any kind of data for a specific user. This system allows you to store arbitrary key-value pairs that are automatically synced across sessions.

## Managing User Attributes

### Get/Set/Remove User Attributes

Store and retrieve custom data for the current user:

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/i3f58-9p/" width="800" height="600" frameborder="0" allowfullscreen></iframe>


## Data Format and Limitations

- **Keys**: Must be strings (e.g., "world_2_unlocked", "player_color")
- **Values**: Must be strings (e.g., "true", "red", "Onion")
- **Complex Data**: Use JSON strings for arrays/objects
- **Automatic Sync**: Data is downloaded on login and synced when updated

## Function Parameters

### Set User Attribute

| Parameter | Type | Description |
|-----------|------|-------------|
| `Attribute Key` | `String` | The name/identifier for the attribute |
| `Attribute Value` | `String` | The value to store |

### Get/Remove User Attribute

| Parameter | Type | Description |
|-----------|------|-------------|
| `Attribute Key` | `String` | The name/identifier of the attribute to retrieve/remove |

## Function Return Values

### Set/Remove User Attribute

| HTTP Status Code | Description |
|------------------|-------------|
| `200` | OK - Attribute updated/removed successfully |
| `400` | Bad request - Invalid key or value |
| `401` | Unauthorized - User not signed in |
| `500` | Unknown server error |

### Get User Attribute

| HTTP Status Code | Description |
|------------------|-------------|
| `200` | OK - Attribute retrieved successfully |
| `401` | Unauthorized - User not signed in |
| `404` | Attribute not found |
| `500` | Unknown server error |

## Next Steps

- [Using Credits](using%20credits.md) - Manage user currency
- [In Game Leaderboard](in%20game%20leaderboard.md) - Track user achievements
- [Friends](friends.md) - Social features and user data
- [Groups](groups.md) - Group-specific user data
