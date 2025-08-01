# Friends System

The GameFuse Friends System allows you to implement social features in your game, such as sending friend requests, accepting or declining requests, and managing a friends list.

## Getting Started with Friends

To use the GameFuse Friends system in Blueprints, you'll need to access the GameFuse Friends subsystem through **Get Game Instance** → **Get Subsystem** → **GameFuse Friends**.

## Sending Friend Requests

You can send a friend request to another user by their username:

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/fozhy3yq/" width="800" height="600" frameborder="0" allowfullscreen></iframe>


## Accepting Friend Requests

When someone sends you a friend request, you can accept it:

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/rqyh-9uy/" width="800" height="600" frameborder="0" allowfullscreen></iframe>



## Declining Friend Requests

You can decline a friend request:

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/rqyh-9uy/" width="800" height="600" frameborder="0" allowfullscreen></iframe>


## Canceling Friend Requests

If you've sent a friend request and want to cancel it:

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/rqyh-9uy/" width="800" height="600" frameborder="0" allowfullscreen></iframe>


## Fetching Friends List

To get the current user's friends list:

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/-8xuz9gt/" width="800" height="600" frameborder="0" allowfullscreen></iframe>


## Fetching Friend Requests

To get pending friend requests:

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/4pm0-r_m/" width="800" height="600" frameborder="0" allowfullscreen></iframe>


## Fetching All Friendship Data

To get comprehensive friendship information including friends, sent requests, and received requests:

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/ze-b8lrs/" width="800" height="600" frameborder="0" allowfullscreen></iframe>


## Unfriending Players

To remove someone from your friends list:

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/19-36k0g/" width="800" height="600" frameborder="0" allowfullscreen></iframe>


## Function Parameters

### Send Friend Request

| Parameter | Type | Description |
|-----------|------|-------------|
| `Username` | `String` | The username of the player to send a friend request to |

### Accept/Decline/Cancel Friend Request & Unfriend

| Parameter | Type | Description |
|-----------|------|-------------|
| `Friendship ID` | `Integer` | The unique ID of the friendship/friend request |

## Function Return Values

### Send Friend Request

| HTTP Status Code | Description |
|------------------|-------------|
| `200` | OK - Friend request sent successfully |
| `400` | Bad request - Invalid username or parameters |
| `401` | Unauthorized - User not signed in |
| `404` | User not found |
| `409` | Conflict - Friend request already exists or users are already friends |
| `500` | Unknown server error |

### Accept/Decline/Cancel Friend Request

| HTTP Status Code | Description |
|------------------|-------------|
| `200` | OK - Action completed successfully |
| `400` | Bad request - Invalid friendship ID |
| `401` | Unauthorized - User not signed in |
| `404` | Friend request not found |
| `500` | Unknown server error |

### Fetch Friends/Requests

| HTTP Status Code | Description |
|------------------|-------------|
| `200` | OK - Data fetched successfully |
| `401` | Unauthorized - User not signed in |
| `500` | Unknown server error |

### Unfriend

| HTTP Status Code | Description |
|------------------|-------------|
| `200` | OK - Friend removed successfully |
| `400` | Bad request - Invalid friendship ID |
| `401` | Unauthorized - User not signed in |
| `404` | Friendship not found |
| `500` | Unknown server error |

## Data Structures

### Friend Request Struct

| Property | Type | Description |
|----------|------|-------------|
| `Friendship ID` | `Integer` | Unique identifier for the friendship |
| `Other User` | `User Struct` | Information about the other user |
| `Status` | `String` | Status of the request (pending, accepted, etc.) |
| `Created At` | `String` | When the request was created |

### User Struct

| Property | Type | Description |
|----------|------|-------------|
| `ID` | `Integer` | Unique user ID |
| `Username` | `String` | User's display name |
| `Email` | `String` | User's email address |
| `Credits` | `Integer` | User's current credit balance |

## Next Steps

- [Groups](groups.md) - Learn about group functionality
- [Chat](chat.md) - Implement messaging between friends
- [Custom User Data](custom%20user%20data.md) - Store additional user information
- [In Game Leaderboard](in%20game%20leaderboard.md) - Show friends' scores 