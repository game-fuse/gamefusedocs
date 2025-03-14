# Chat System

The GameFuse Chat System allows you to implement real-time chat functionality in your game. This includes creating chats, sending messages, and managing message read status.

## Getting Started with Chat

To use the GameFuse Chat system in Blueprints, you'll need to access the GameFuse Chat subsystem:


<!-- 
<!-- <iframe src="https://blueprintue.com/render/your-blueprint-id/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

[Copy Code](https://blueprintue.com/blueprint/your-blueprint-id/) --> -->

## Creating a Chat

You can create a new chat with one or more participants:


<!-- 
<!-- <iframe src="https://blueprintue.com/render/your-blueprint-id/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

[Copy Code](https://blueprintue.com/blueprint/your-blueprint-id/) --> -->

The Blueprint should:

1. Get the GameFuse Chat subsystem
2. Create an array of participant usernames
3. Set an initial message
4. Call CreateChat with the usernames and initial message
5. Create a callback function to handle the response
6. Process the created chat data

## Sending Messages

Once you have a chat, you can send messages to it:


<!-- 
<!-- <iframe src="https://blueprintue.com/render/your-blueprint-id/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

[Copy Code](https://blueprintue.com/blueprint/your-blueprint-id/) --> -->

The Blueprint should:

1. Get the GameFuse Chat subsystem
2. Call SendMessage with the chat ID and message text
3. Create a callback function to handle the response
4. Process the sent message data

## Marking Messages as Read

You can mark messages as read to keep track of which messages have been seen:


<!-- 
<!-- <iframe src="https://blueprintue.com/render/your-blueprint-id/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

[Copy Code](https://blueprintue.com/blueprint/your-blueprint-id/) --> -->

The Blueprint should:

1. Get the GameFuse Chat subsystem
2. Call MarkMessageAsRead with the message ID
3. Create a callback function to handle the response
4. Process the success or failure of marking the message as read

## Fetching Chats and Messages

You can fetch all chats for the current user:


<!-- 
<!-- <iframe src="https://blueprintue.com/render/your-blueprint-id/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

[Copy Code](https://blueprintue.com/blueprint/your-blueprint-id/) --> -->

The Blueprint should:

1. Get the GameFuse Chat subsystem
2. Call FetchAllChats with page number (e.g., 1)
3. Create a callback function to handle the response
4. Process the array of chats from ChatSystem->GetAllChats()

You can also fetch messages for a specific chat:


<!-- 
<!-- <iframe src="https://blueprintue.com/render/your-blueprint-id/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

[Copy Code](https://blueprintue.com/blueprint/your-blueprint-id/) --> -->

The Blueprint should:

1. Get the GameFuse Chat subsystem
2. Call FetchMessages with the chat ID and page number
3. Create a callback function to handle the response
4. Process the array of messages from ChatSystem->GetChatMessages()

## Clearing Chat Data

You can clear the cached chat data when it's no longer needed:


<!-- 
<!-- <iframe src="https://blueprintue.com/render/your-blueprint-id/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

[Copy Code](https://blueprintue.com/blueprint/your-blueprint-id/) --> -->

The Blueprint should:

1. Get the GameFuse Chat subsystem
2. Call ClearChatData
3. Update any UI elements that were displaying chat data

## Function Return Values

### Chat Creation

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK |
| `400`            | Missing or invalid parameters |
| `401`            | User not authenticated |
| `500`            | Unknown server error |

### Message Operations

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK |
| `400`            | Missing or invalid parameters |
| `401`            | User not authenticated or not a participant in the chat |
| `404`            | Chat or message not found |
| `500`            | Unknown server error |

### Fetching Data

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK |
| `401`            | User not authenticated |
| `500`            | Unknown server error | 