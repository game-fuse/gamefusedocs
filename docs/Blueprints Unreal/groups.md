# Groups System

The GameFuse Groups System allows you to create and manage groups of users in your game. This includes creating groups, joining groups, managing group membership, and handling group attributes.

## Getting Started with Groups

To use the GameFuse Groups system in Blueprints, you'll need to access the GameFuse Groups subsystem through **Get Game Instance** → **Get Subsystem** → **GameFuse Groups**.

## Creating a Group

You can create a new group with customizable properties:

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/1idvg84b/" width="800" height="600" frameborder="0" allowfullscreen></iframe>


## Fetching Groups

### Search/Fetch All Groups

Get all available groups or search for specific groups:

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/z47mewgv/" width="800" height="600" frameborder="0" allowfullscreen></iframe>


### Fetch Specific Group

Get detailed information about a specific group:

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/z47mewgv/" width="800" height="600" frameborder="0" allowfullscreen></iframe>


## Joining Groups

### Join Public Groups

Join a public group directly:

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/6-1o0k7s/" width="800" height="600" frameborder="0" allowfullscreen></iframe>


### Request to Join Private Groups

Request to join a private or invite-only group:

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/6-1o0k7s/" width="800" height="600" frameborder="0" allowfullscreen></iframe>


## Managing Join Requests

### Accept Join Requests

As a group admin, accept pending join requests:

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/g_elf_as/" width="800" height="600" frameborder="0" allowfullscreen></iframe>


### Decline Join Requests

As a group admin, decline pending join requests:

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/g_elf_as/" width="800" height="600" frameborder="0" allowfullscreen></iframe>


## Leaving Groups

Leave a group you're currently a member of:

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/cwhosp1g/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

## Deleting Groups

As a group owner, delete a group:

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/tfeb8hus/" width="800" height="600" frameborder="0" allowfullscreen></iframe>


## Managing Group Admins

### Add/Remove Group Admins

Manage administrative privileges within the group:

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/52pf4emg/" width="800" height="600" frameborder="0" allowfullscreen></iframe>


## Managing Group Attributes

### Set/Get Group Attributes

Store and retrieve custom data for groups:

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/ylrsthg4/" width="800" height="600" frameborder="0" allowfullscreen></iframe>


## Function Parameters

### Create Group

| Parameter | Type | Description |
|-----------|------|-------------|
| `Group Name` | `String` | The name of the group |
| `Group Type` | `String` | Type: "public", "private", or "invite-only" |
| `Max Group Size` | `Integer` | Maximum number of members (optional) |
| `Description` | `String` | Group description (optional) |

### Join/Leave/Delete Group

| Parameter | Type | Description |
|-----------|------|-------------|
| `Group ID` | `Integer` | Unique identifier of the group |

### Manage Members/Admins

| Parameter | Type | Description |
|-----------|------|-------------|
| `Group ID` | `Integer` | Unique identifier of the group |
| `User ID` | `Integer` | Unique identifier of the user |

### Group Attributes

| Parameter | Type | Description |
|-----------|------|-------------|
| `Group ID` | `Integer` | Unique identifier of the group |
| `Attribute Key` | `String` | Name of the attribute |
| `Attribute Value` | `String` | Value to store |

## Function Return Values

### Create Group

| HTTP Status Code | Description |
|------------------|-------------|
| `200` | OK - Group created successfully |
| `400` | Bad request - Invalid parameters |
| `401` | Unauthorized - User not signed in |
| `409` | Conflict - Group name already exists |
| `500` | Unknown server error |

### Join/Leave Group

| HTTP Status Code | Description |
|------------------|-------------|
| `200` | OK - Action completed successfully |
| `400` | Bad request - Invalid group ID |
| `401` | Unauthorized - User not signed in |
| `403` | Forbidden - Not allowed to perform action |
| `404` | Group not found |
| `500` | Unknown server error |

### Manage Group

| HTTP Status Code | Description |
|------------------|-------------|
| `200` | OK - Management action completed |
| `400` | Bad request - Invalid parameters |
| `401` | Unauthorized - User not signed in |
| `403` | Forbidden - Insufficient permissions |
| `404` | Group or user not found |
| `500` | Unknown server error |

## Data Structures

### Group Struct

| Property | Type | Description |
|----------|------|-------------|
| `ID` | `Integer` | Unique group identifier |
| `Name` | `String` | Group name |
| `Description` | `String` | Group description |
| `Group Type` | `String` | Type of group (public, private, invite-only) |
| `Max Group Size` | `Integer` | Maximum number of members |
| `Current Size` | `Integer` | Current number of members |
| `Owner ID` | `Integer` | User ID of the group owner |
| `Created At` | `String` | When the group was created |
| `Members` | `Array` | List of group members |
| `Admins` | `Array` | List of group administrators |

### Group Member Struct

| Property | Type | Description |
|----------|------|-------------|
| `User ID` | `Integer` | Unique user identifier |
| `Username` | `String` | User's display name |
| `Role` | `String` | Member role (owner, admin, member) |
| `Joined At` | `String` | When the user joined the group |

## Next Steps

- [Chat](chat.md) - Implement group chat functionality
- [Friends](friends.md) - Invite friends to groups
- [In Game Leaderboard](in%20game%20leaderboard.md) - Create group leaderboards
- [Custom User Data](custom%20user%20data.md) - Store group-related user data 