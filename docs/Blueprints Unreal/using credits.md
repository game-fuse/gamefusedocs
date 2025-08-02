# Using Credits

Credits are GameFuse's virtual currency system. Each user has a credit balance that can be used to purchase store items, unlock features, or power your game's economy.

## Overview

Credits are a numeric attribute of each game user stored as a simple integer value. They can be added manually through API calls and are automatically deducted when users purchase store items.

## Getting User Credits

You can retrieve the current user's credit balance:

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/45msnril/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

## Adding Credits

You can add credits to the current user's account:

!!! example "Blueprint Example"  
    <iframe src="https://blueprintue.com/render/45msnril/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

## Subtracting Credits

You can subtract credits from the current user's account:

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/45msnril/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

## Setting Credits

You can set the user's credits to a specific amount:

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/45msnril/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

    **Blueprint Implementation:**
    1. Get the **GameFuse User** subsystem
    2. Call **Set Credits** with the target amount
    3. Connect to the **On Success** or **On Failure** execution pins
    4. The **On Success** pin provides the new credit balance

## Function Parameters

### Add Credits

| Parameter | Type | Description |
|-----------|------|-------------|
| `Credits` | `Integer` | The amount of credits to add (must be positive) |

### Subtract Credits

| Parameter | Type | Description |
|-----------|------|-------------|
| `Credits` | `Integer` | The amount of credits to subtract (must be positive) |

### Set Credits

| Parameter | Type | Description |
|-----------|------|-------------|
| `Credits` | `Integer` | The target credit amount to set (must be non-negative) |

## Function Return Values

### Add/Subtract/Set Credits

| HTTP Status Code | Description |
|------------------|-------------|
| `200` | OK - Credits updated successfully |
| `400` | Bad request - Invalid parameters (negative values, etc.) |
| `401` | Unauthorized - User not signed in |
| `403` | Forbidden - Insufficient credits (for subtract operation) |
| `500` | Unknown server error |

## Next Steps

- [Using the Store](using%20the%20store%20in%20your%20game.md) - Learn how credits integrate with purchases
- [Custom User Data](custom%20user%20data.md) - Store additional user information
- [In Game Leaderboard](in%20game%20leaderboard.md) - Display user achievements and rankings
