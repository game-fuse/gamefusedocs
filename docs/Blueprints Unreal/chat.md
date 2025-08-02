# Chat System

The GameFuse Chat System allows you to implement real-time chat functionality in your game. This includes creating chats, sending messages, and managing message read status.

## Getting Started with Chat

To use the GameFuse Chat system in Blueprints, you'll need to access the GameFuse Chat subsystem through **Get Game Instance** → **Get Subsystem** → **GameFuse Chat**.

## Creating a Chat

Create a new chat conversation with multiple participants:

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/2wo07x91/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

## Fetching Chats and Messages

### Fetch All Chats

Get all chats the current user is participating in:

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/53u4e0xf/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

### Fetch Messages for a Chat

Get all messages in a specific chat:

!!! example "Blueprint Example" 
    <iframe src="https://blueprintue.com/render/53u4e0xf/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

## Sending Messages

Send a message in an existing chat:

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/zx6megax/" width="800" height="600" frameborder="0" allowfullscreen></iframe>


## Marking Messages as Read

Mark messages as read to track message status:

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/zx6megax/" width="800" height="600" frameborder="0" allowfullscreen></iframe>


## Function Parameters

### Create Chat

| Parameter | Type | Description |
|-----------|------|-------------|
| `Participant Usernames` | `Array of Strings` | Usernames of users to include in the chat |
| `Initial Message` | `String` | Optional first message for the chat |

### Send Message

| Parameter | Type | Description |
|-----------|------|-------------|
| `Chat ID` | `Integer` | Unique identifier of the chat |
| `Message Text` | `String` | The message content to send |

### Fetch/Mark Messages

| Parameter | Type | Description |
|-----------|------|-------------|
| `Chat ID` | `Integer` | Unique identifier of the chat (for fetching) |
| `Message ID` | `Integer` | Unique identifier of the message (for marking as read) |

## Function Return Values

### Create Chat

| HTTP Status Code | Description |
|------------------|-------------|
| `200` | OK - Chat created successfully |
| `400` | Bad request - Invalid parameters or usernames |
| `401` | Unauthorized - User not signed in |
| `404` | One or more users not found |
| `500` | Unknown server error |

### Send Message

| HTTP Status Code | Description |
|------------------|-------------|
| `200` | OK - Message sent successfully |
| `400` | Bad request - Invalid chat ID or empty message |
| `401` | Unauthorized - User not signed in |
| `403` | Forbidden - User not participant in chat |
| `404` | Chat not found |
| `500` | Unknown server error |

### Fetch Chats/Messages

| HTTP Status Code | Description |
|------------------|-------------|
| `200` | OK - Data fetched successfully |
| `401` | Unauthorized - User not signed in |
| `404` | Chat not found (for specific chat fetch) |
| `500` | Unknown server error |

### Mark Message as Read

| HTTP Status Code | Description |
|------------------|-------------|
| `200` | OK - Message marked as read |
| `400` | Bad request - Invalid message ID |
| `401` | Unauthorized - User not signed in |
| `404` | Message not found |
| `500` | Unknown server error |

## Data Structures

### Chat Struct

| Property | Type | Description |
|----------|------|-------------|
| `ID` | `Integer` | Unique chat identifier |
| `Participants` | `Array` | List of chat participants |
| `Last Message` | `Message Struct` | Most recent message in the chat |
| `Unread Count` | `Integer` | Number of unread messages for current user |
| `Created At` | `String` | When the chat was created |
| `Updated At` | `String` | When the chat was last updated |

### Message Struct

| Property | Type | Description |
|----------|------|-------------|
| `ID` | `Integer` | Unique message identifier |
| `Chat ID` | `Integer` | ID of the chat this message belongs to |
| `Sender Username` | `String` | Username of the message sender |
| `Sender ID` | `Integer` | User ID of the message sender |
| `Message Text` | `String` | Content of the message |
| `Is Read` | `Boolean` | Whether the current user has read this message |
| `Created At` | `String` | When the message was sent |

### Chat Participant Struct

| Property | Type | Description |
|----------|------|-------------|
| `User ID` | `Integer` | Unique user identifier |
| `Username` | `String` | User's display name |
| `Last Read At` | `String` | When the user last read messages in this chat |

## Next Steps

- [Friends](friends.md) - Create chats with friends
- [Groups](groups.md) - Implement group chat functionality
- [Custom User Data](custom%20user%20data.md) - Store chat preferences
- [Using Credits](using%20credits.md) - Premium chat features 