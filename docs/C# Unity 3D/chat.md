# Chat System

GameFuse provides a flexible chat system that allows players to communicate directly with each other and in groups. The messaging functionality is accessible through the authenticated `GameFuseUser.CurrentUser` instance.

## Getting Started

All chat methods require user authentication. Ensure you have a signed-in user before calling any chat methods:

!!! example
    ```csharp
    if (GameFuseUser.CurrentUser == null)
    {
        Debug.LogError("User must be authenticated first");
        return;
    }
    ```

## Fetching Chats

Use `FetchMyPaginatedChatsAsync` to retrieve all chats the current user is part of:

!!! example
    ```csharp
    async void LoadMyChats()
    {
        try
        {
            var response = await GameFuseUser.CurrentUser.FetchMyPaginatedChatsAsync(page: 1);
            
            Debug.Log($"Found {response.Chats.Count} chats");
            Debug.Log($"Current page: {response.CurrentPage}, Total pages: {response.TotalPages}");
            
            foreach (var chat in response.Chats)
            {
                Debug.Log($"Chat ID: {chat.Id}");
                Debug.Log($"Chat Type: {chat.ChatType}");
                Debug.Log($"Participants: {chat.Participants.Count}");
                
                // Show latest message if available
                if (chat.Messages?.Count > 0)
                {
                    var latestMessage = chat.Messages.First();
                    Debug.Log($"Latest: {latestMessage.Text} (from user {latestMessage.UserId})");
                }
            }
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Failed to load chats: {ex.Message}");
        }
    }
    ```

## Creating Chats

### Create Direct Chat

Use `CreateDirectChatAsync` to start a direct conversation with other users:

!!! example
    ```csharp
    async void CreateDirectChat()
    {
        try
        {
            var usernames = new List<string> { "playerUsername", "anotherPlayer" };
            string initialMessage = "Hey! Want to team up for the next match?";
            
            var chat = await GameFuseUser.CurrentUser.CreateDirectChatAsync(usernames, initialMessage);
            
            Debug.Log($"Direct chat created! Chat ID: {chat.Id}");
            Debug.Log($"Participants: {chat.Participants.Count}");
            
            // The initial message is automatically sent
            if (chat.Messages?.Count > 0)
            {
                Debug.Log($"Initial message sent: {chat.Messages.First().Text}");
            }
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Failed to create direct chat: {ex.Message}");
        }
    }
    ```

### Create Group Chat

Use `CreateGroupChatAsync` to create a chat for a specific group:

!!! example
    ```csharp
    async void CreateGroupChat(int groupId)
    {
        try
        {
            string initialMessage = "Welcome to our group chat! Let's coordinate our strategy.";
            
            var chat = await GameFuseUser.CurrentUser.CreateGroupChatAsync(groupId, initialMessage);
            
            Debug.Log($"Group chat created! Chat ID: {chat.Id}");
            Debug.Log($"Chat type: {chat.ChatType}");
            Debug.Log($"Group members in chat: {chat.Participants.Count}");
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Failed to create group chat: {ex.Message}");
        }
    }
    ```

## Retrieving Messages

Use `FetchPaginatedMessagesForChatAsync` to get messages from a specific chat:

!!! example
    ```csharp
    async void LoadChatMessages(int chatId)
    {
        try
        {
            var response = await GameFuseUser.CurrentUser.FetchPaginatedMessagesForChatAsync(
                chatId, 
                page: 1);
                
            Debug.Log($"Retrieved {response.Messages.Count} messages");
            Debug.Log($"Page {response.CurrentPage} of {response.TotalPages}");
            
            foreach (var message in response.Messages)
            {
                Debug.Log($"Message ID: {message.Id}");
                Debug.Log($"From User: {message.UserId}");
                Debug.Log($"Text: {message.Text}");
                Debug.Log($"Sent: {message.CreatedAt}");
                Debug.Log($"Read by current user: {message.ReadByCurrentUser}");
            }
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Failed to load messages: {ex.Message}");
        }
    }
    
    async void LoadOlderMessages(int chatId, int page)
    {
        try
        {
            var response = await GameFuseUser.CurrentUser.FetchPaginatedMessagesForChatAsync(chatId, page);
            
            // Handle older messages...
            Debug.Log($"Loaded page {page} with {response.Messages.Count} older messages");
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Failed to load older messages: {ex.Message}");
        }
    }
    ```

## Sending Messages

Use `SendMessageToChatAsync` to send a message to an existing chat:

!!! example
    ```csharp
    async void SendMessage(int chatId, string messageText)
    {
        try
        {
            var message = await GameFuseUser.CurrentUser.SendMessageToChatAsync(chatId, messageText);
            
            Debug.Log($"Message sent! Message ID: {message.Id}");
            Debug.Log($"Message text: {message.Text}");
            Debug.Log($"Sent at: {message.CreatedAt}");
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Failed to send message: {ex.Message}");
        }
    }
    ```

## Marking Messages as Read

Use `MarkMessageAsReadAsync` to mark messages as read:

!!! example
    ```csharp
    async void MarkMessageAsRead(int messageId)
    {
        try
        {
            var response = await GameFuseUser.CurrentUser.MarkMessageAsReadAsync(messageId);
            Debug.Log($"Message marked as read: {response.Message}");
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Failed to mark message as read: {ex.Message}");
        }
    }
    
    async void MarkAllMessagesAsRead(List<Message> messages)
    {
        try
        {
            foreach (var message in messages)
            {
                if (!message.ReadByCurrentUser)
                {
                    await GameFuseUser.CurrentUser.MarkMessageAsReadAsync(message.Id);
                }
            }
            Debug.Log("All messages marked as read");
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Failed to mark messages as read: {ex.Message}");
        }
    }
    ```

## Complete Example

Here's a comprehensive example showing how to implement a chat system:

!!! example
    ```csharp
    using System.Collections.Generic;
    using UnityEngine;
    using UnityEngine.UI;
    using GameFuse;
    using GameFuse.Models.Shared;
    using System.Linq;
    
    public class ChatManager : MonoBehaviour
    {
        [Header("UI References")]
        public Transform chatsListParent;
        public Transform messagesListParent;
        public GameObject chatItemPrefab;
        public GameObject messageItemPrefab;
        public InputField messageInput;
        public Button sendButton;
        public Text chatTitleText;
        public Button createDirectChatButton;
        
        [Header("Chat Creation")]
        public InputField usernameInput;
        
        private Chat currentChat;
        private List<Message> currentMessages = new List<Message>();
        
        void Start()
        {
            sendButton.onClick.AddListener(OnSendMessageClicked);
            createDirectChatButton.onClick.AddListener(OnCreateDirectChatClicked);
            
            RefreshChatsList();
        }
        
        async void OnSendMessageClicked()
        {
            if (currentChat == null || string.IsNullOrEmpty(messageInput.text))
                return;
                
            await SendMessage(messageInput.text);
        }
        
        async void OnCreateDirectChatClicked()
        {
            string username = usernameInput.text.Trim();
            if (string.IsNullOrEmpty(username))
                return;
                
            await CreateDirectChat(username);
        }
        
        public async System.Threading.Tasks.Task RefreshChatsList()
        {
            try
            {
                var response = await GameFuseUser.CurrentUser.FetchMyPaginatedChatsAsync(1);
                
                // Clear existing items
                foreach (Transform child in chatsListParent)
                {
                    Destroy(child.gameObject);
                }
                
                // Create UI items for each chat
                foreach (var chat in response.Chats)
                {
                    var chatItem = Instantiate(chatItemPrefab, chatsListParent);
                    var chatUI = chatItem.GetComponent<ChatItemUI>();
                    if (chatUI != null)
                    {
                        chatUI.Setup(chat, this);
                    }
                }
                
                Debug.Log($"Loaded {response.Chats.Count} chats");
            }
            catch (System.Exception ex)
            {
                Debug.LogError($"Error refreshing chats: {ex.Message}");
            }
        }
        
        public async System.Threading.Tasks.Task SelectChat(Chat chat)
        {
            currentChat = chat;
            chatTitleText.text = GetChatDisplayName(chat);
            
            await LoadChatMessages(chat.Id);
        }
        
        async System.Threading.Tasks.Task LoadChatMessages(int chatId)
        {
            try
            {
                var response = await GameFuseUser.CurrentUser.FetchPaginatedMessagesForChatAsync(chatId, 1);
                
                currentMessages.Clear();
                currentMessages.AddRange(response.Messages);
                
                // Clear existing message items
                foreach (Transform child in messagesListParent)
                {
                    Destroy(child.gameObject);
                }
                
                // Create UI items for each message
                foreach (var message in currentMessages)
                {
                    var messageItem = Instantiate(messageItemPrefab, messagesListParent);
                    var messageUI = messageItem.GetComponent<MessageItemUI>();
                    if (messageUI != null)
                    {
                        bool isFromCurrentUser = message.UserId == GameFuseUser.CurrentUser.Id;
                        messageUI.Setup(message, isFromCurrentUser);
                    }
                }
                
                // Mark unread messages as read
                await MarkUnreadMessagesAsRead();
            }
            catch (System.Exception ex)
            {
                Debug.LogError($"Error loading messages: {ex.Message}");
            }
        }
        
        async System.Threading.Tasks.Task MarkUnreadMessagesAsRead()
        {
            try
            {
                foreach (var message in currentMessages)
                {
                    if (!message.ReadByCurrentUser)
                    {
                        await GameFuseUser.CurrentUser.MarkMessageAsReadAsync(message.Id);
                    }
                }
            }
            catch (System.Exception ex)
            {
                Debug.LogError($"Error marking messages as read: {ex.Message}");
            }
        }
        
        public async System.Threading.Tasks.Task SendMessage(string messageText)
        {
            try
            {
                var sentMessage = await GameFuseUser.CurrentUser.SendMessageToChatAsync(
                    currentChat.Id, 
                    messageText);
                    
                // Add to current messages
                currentMessages.Add(sentMessage);
                
                // Create UI for new message
                var messageItem = Instantiate(messageItemPrefab, messagesListParent);
                var messageUI = messageItem.GetComponent<MessageItemUI>();
                if (messageUI != null)
                {
                    messageUI.Setup(sentMessage, true); // true = from current user
                }
                
                // Clear input
                messageInput.text = "";
                
                Debug.Log("Message sent successfully");
            }
            catch (System.Exception ex)
            {
                Debug.LogError($"Error sending message: {ex.Message}");
            }
        }
        
        public async System.Threading.Tasks.Task CreateDirectChat(string username)
        {
            try
            {
                var usernames = new List<string> { username };
                string initialMessage = "Hello! I'd like to chat with you.";
                
                var chat = await GameFuseUser.CurrentUser.CreateDirectChatAsync(usernames, initialMessage);
                
                // Clear input
                usernameInput.text = "";
                
                // Refresh chats list and select new chat
                await RefreshChatsList();
                await SelectChat(chat);
                
                Debug.Log($"Created direct chat with {username}");
            }
            catch (System.Exception ex)
            {
                Debug.LogError($"Error creating direct chat: {ex.Message}");
            }
        }
        
        public async System.Threading.Tasks.Task CreateGroupChat(int groupId)
        {
            try
            {
                string initialMessage = "Welcome to our group chat!";
                
                var chat = await GameFuseUser.CurrentUser.CreateGroupChatAsync(groupId, initialMessage);
                
                // Refresh chats list and select new chat
                await RefreshChatsList();
                await SelectChat(chat);
                
                Debug.Log($"Created group chat for group {groupId}");
            }
            catch (System.Exception ex)
            {
                Debug.LogError($"Error creating group chat: {ex.Message}");
            }
        }
        
        string GetChatDisplayName(Chat chat)
        {
            if (chat.ChatType == "direct")
            {
                // Find other participants (not current user)
                var otherParticipants = chat.Participants.Where(p => p.Id != GameFuseUser.CurrentUser.Id);
                return string.Join(", ", otherParticipants.Select(p => p.Username));
            }
            else
            {
                return $"Group Chat ({chat.Participants.Count} members)";
            }
        }
    }
    
    public class ChatItemUI : MonoBehaviour
    {
        public Text chatNameText;
        public Text lastMessageText;
        public Button selectButton;
        
        private Chat chat;
        private ChatManager manager;
        
        void Start()
        {
            selectButton.onClick.AddListener(() => manager.SelectChat(chat));
        }
        
        public void Setup(Chat chatData, ChatManager chatManager)
        {
            chat = chatData;
            manager = chatManager;
            
            // Set chat name
            chatNameText.text = manager.GetChatDisplayName(chat);
            
            // Set last message preview
            if (chat.Messages?.Count > 0)
            {
                var lastMessage = chat.Messages.First();
                bool isFromCurrentUser = lastMessage.UserId == GameFuseUser.CurrentUser.Id;
                string sender = isFromCurrentUser ? "You" : "Other";
                lastMessageText.text = $"{sender}: {lastMessage.Text}";
            }
            else
            {
                lastMessageText.text = "No messages yet";
            }
        }
    }
    
    public class MessageItemUI : MonoBehaviour
    {
        public Text messageText;
        public Text timestampText;
        public GameObject currentUserBubble;
        public GameObject otherUserBubble;
        
        public void Setup(Message message, bool isFromCurrentUser)
        {
            messageText.text = message.Text;
            
            // Format timestamp
            if (System.DateTime.TryParse(message.CreatedAt, out System.DateTime dateTime))
            {
                timestampText.text = dateTime.ToString("HH:mm");
            }
            
            // Show appropriate bubble
            currentUserBubble.SetActive(isFromCurrentUser);
            otherUserBubble.SetActive(!isFromCurrentUser);
        }
    }
    ```

## Method Reference

### `FetchMyPaginatedChatsAsync`
Retrieves a paginated list of chats the current user is part of.

**Parameters:**
- `page` (int, optional): Page number for pagination (default 1)
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<PaginatedChatsResponse>` - Paginated list of chats with metadata

### `CreateDirectChatAsync`
Creates a new direct chat with specified users and sends an initial message.

**Parameters:**
- `usernames` (List<string>): List of usernames to add to the chat
- `initialMessageText` (string): First message to send in the chat
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<Chat>` - The created chat with initial message

### `CreateGroupChatAsync`
Creates a new group chat for a specific group and sends an initial message.

**Parameters:**
- `groupId` (int): ID of the group to create a chat for
- `initialMessageText` (string): First message to send in the chat
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<Chat>` - The created group chat with initial message

### `FetchPaginatedMessagesForChatAsync`
Retrieves a paginated list of messages for a specific chat.

**Parameters:**
- `chatId` (int): ID of the chat to fetch messages for
- `page` (int, optional): Page number for pagination (default 1)
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<PaginatedMessagesResponse>` - Paginated list of messages

### `SendMessageToChatAsync`
Sends a message to an existing chat.

**Parameters:**
- `chatId` (int): ID of the chat to send the message to
- `messageText` (string): Text content of the message
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<Message>` - The newly created message

### `MarkMessageAsReadAsync`
Marks a specific message as read by the current user.

**Parameters:**
- `messageId` (int): ID of the message to mark as read
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<MarkAsReadResponse>` - Response confirming the message was marked as read

## Data Models

### Chat
Contains information about a chat conversation:
- `Id` (int): Chat identifier
- `ChatType` (string): Type of chat ("direct" or "group")
- `Participants` (List<User>): List of chat participants
- `Messages` (List<Message>): Recent messages in the chat

### Message
Contains information about an individual message:
- `Id` (int): Message identifier
- `Text` (string): Message content
- `UserId` (int): ID of the user who sent the message
- `CreatedAt` (string): Timestamp when the message was created
- `ReadByCurrentUser` (bool): Whether the current user has read the message

### PaginatedChatsResponse
Response from fetching chats:
- `Chats` (List<Chat>): List of chats for the current page
- `CurrentPage` (int): Current page number
- `TotalPages` (int): Total number of pages
- `TotalCount` (int): Total number of chats

### PaginatedMessagesResponse
Response from fetching messages:
- `Messages` (List<Message>): List of messages for the current page
- `CurrentPage` (int): Current page number
- `TotalPages` (int): Total number of pages
- `TotalCount` (int): Total number of messages

## Error Handling

All chat methods are async and may throw exceptions. Always wrap calls in try-catch blocks:

- **Network connectivity issues** - Connection problems
- **Authentication errors** - User not signed in
- **Invalid parameters** - User not found, chat not found, empty message text
- **Permission errors** - Attempting to access chats the user isn't part of
- **Server errors** - Temporary GameFuse service issues

## Function return values

### HTTP Status Codes

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK - Operation completed successfully |
| `400`            | Bad Request - Invalid parameters (empty usernames, invalid chat ID) |
| `401`            | Unauthorized - User not authenticated |
| `403`            | Forbidden - Insufficient permissions |
| `404`            | Not Found - Chat or user not found |
| `500`            | Internal Server Error - Server error |

## Important Notes

- **Pagination**: All fetch methods support pagination to handle large amounts of data efficiently
- **Initial Messages**: Both direct and group chat creation methods require an initial message
- **Real-time Updates**: Consider implementing polling or websockets for real-time message updates
- **Read Status**: Track message read status to provide better user experience
- **Chat Types**: Direct chats are for person-to-person communication, group chats are tied to specific groups