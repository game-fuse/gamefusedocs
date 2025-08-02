# Using the Store in Your Game

The GameFuse Store system allows you to create, manage, and purchase store items in your game. Store items are fetched when you call `Fetch Store Items` and are refreshed every time you call it again.

## Getting Started with Store

To use the GameFuse Store system, you'll need to access the `GameFuse Manager` and `GameFuse User` subsystems through Blueprint nodes.

## Fetching Available Store Items

Store items are fetched when you call `Fetch Store Items`. The items will be refreshed every time you call it again.

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/mxg5x-ii/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

## Fetching Purchased Store Items

### Fetching Current User's Purchased Store Items

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/1goi60ut/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

### Fetching Other Users' Purchased Store Items

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/1goi60ut/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

## Purchasing Store Items

To purchase a store item, you can use the store item ID. If the user doesn't have enough credits, the purchase will fail.

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/sjv71g-n/" width="800" height="600" frameborder="0" allowfullscreen></iframe>


## Removing Store Items

You can remove a store item from a user's inventory:

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/44-lel-u/" width="800" height="600" frameborder="0" allowfullscreen></iframe>


## Function Parameters

### Fetching Store Items

| Parameter | Type | Description |
|-----------|------|-------------|
| `User ID` | `Integer` | The user ID to fetch purchased items for (optional, for user-specific fetch) |

### Purchasing and Removing Store Items

| Parameter | Type | Description |
|-----------|------|-------------|
| `Store Item ID` | `Integer` | The ID of the store item to purchase or remove |

## Function Return Values

### `Purchase Store Item`

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK - Item purchased successfully |
| `400`            | Bad request - Invalid parameters |
| `401`            | Unauthorized - User not signed in |
| `403`            | Not enough credits or item already purchased |
| `404`            | Item not found |
| `500`            | Unknown server error |

### `Remove Store Item`

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK - Item removed successfully |
| `400`            | Bad request - Invalid parameters |
| `401`            | Unauthorized - User not signed in |
| `404`            | Item not found or not owned by user |
| `500`            | Unknown server error |

### `Fetch My Purchased Store Items` / `Fetch User Purchased Store Items`

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK - Purchased items fetched successfully |
| `401`            | Unauthorized - User not signed in |
| `404`            | User not found (for user-specific fetch) |
| `500`            | Unknown server error |

## Cached Data Access

You can access cached store data without making API calls:

!!! example "Blueprint Example"
    **Blueprint Implementation:**
    1. **Get Cached Available Store Items:**
       - **Get Game Instance** → **Get Subsystem** → **GameFuse Manager** → **Get Game Store Items**
       - Returns array of `Store Item` structs

    2. **Get Cached Purchased Store Items:**
       - **Get Game Instance** → **Get Subsystem** → **GameFuse User** → **Get Purchased Store Items**
       - Returns array of `Store Item` structs

## Store Item Struct Reference

The `Store Item` struct contains the following properties:

| Property | Type | Description |
|----------|------|-------------|
| `ID` | `Integer` | Unique identifier for the store item |
| `Name` | `String` | Display name of the store item |
| `Description` | `String` | Detailed description of the store item |
| `Category` | `String` | Category classification of the item |
| `Cost` | `Integer` | Cost in credits to purchase the item |
| `Icon URL` | `String` | URL to the item's icon image |

