# Chat System

GameFuse provides a flexible chat system that allows players to communicate directly with each other and in groups. The `GameFuseUser.Chats` partial class contains methods to create and manage chats and messages within your game.

## Chat System Methods

All chat methods are accessible through the `GameFuseUser` instance after a successful login.

### GetChatsAsync

Retrieves a paginated list of both direct and group chats for the current user.

**Parameters:**
- `int page = 1` (optional): Page number for pagination

**Returns:**
- `Task<GetChatsResponse>`: Response containing direct and group chats

**Example:**
```csharp
try
{
    // Get the current authenticated user
    GameFuseUser currentUser = GameFuseUser.CurrentUser;
    
    // Get the user's chats (first page)
    GetChatsResponse chatsResponse = await currentUser.GetChatsAsync();
    
    // Access direct chats
    Chat[] directChats = chatsResponse.DirectChats;
    
    // Access group chats
    Chat[] groupChats = chatsResponse.GroupChats;
    
    // Display the chats
    Debug.Log($"Found {directChats.Length} direct chats and {groupChats.Length} group chats");
    
    foreach (Chat chat in directChats)
    {
        string participantNames = string.Join(", ", chat.Participants.Select(p => p.Username));
        Debug.Log($"Direct Chat ID: {chat.Id}, Participants: {participantNames}");
    }
}
catch (ApiException ex)
{
    Debug.LogError($"Failed to get chats: {ex.Message}");
}
```

### CreateDirectChatAsync

Creates a new direct chat with specified users and sends the first message.

**Parameters:**
- `string[] usernames` (required): Array of usernames to add to the chat
- `string text` (required): Initial message text to send

**Returns:**
- `Task<Chat>`: Response containing the created chat

**Example:**
```csharp
try
{
    // Get the current authenticated user
    GameFuseUser currentUser = GameFuseUser.CurrentUser;
    
    // Create a direct chat with another user
    string[] usernames = new string[] { "playerUsername" };
    string initialMessage = "Hello! Want to play a game?";
    
    Chat newChat = await currentUser.CreateDirectChatAsync(usernames, initialMessage);
    
    // Access the new chat ID
    int chatId = newChat.Id;
    Debug.Log($"Direct chat created successfully! Chat ID: {chatId}");
    
    // Access the first message
    if (newChat.Messages != null && newChat.Messages.Length > 0)
    {
        Debug.Log($"First message: {newChat.Messages[0].Text}");
    }
}
catch (ApiException ex)
{
    Debug.LogError($"Failed to create direct chat: {ex.Message}");
}
```

### CreateGroupChatAsync

Creates a new group chat for a specific group and sends the first message.

**Parameters:**
- `int groupId` (required): ID of the group to create a chat for
- `string text` (required): Initial message text to send

**Returns:**
- `Task<Chat>`: Response containing the created chat

**Example:**
```csharp
try
{
    // Get the current authenticated user
    GameFuseUser currentUser = GameFuseUser.CurrentUser;
    
    // Create a chat for a specific group
    int groupId = 123;
    string initialMessage = "Hello group members! Let's coordinate our game strategy.";
    
    Chat groupChat = await currentUser.CreateGroupChatAsync(groupId, initialMessage);
    
    // Access the new chat ID
    int chatId = groupChat.Id;
    Debug.Log($"Group chat created successfully! Chat ID: {chatId}");
    
    // Access participants
    if (groupChat.Participants != null)
    {
        Debug.Log($"Group chat has {groupChat.Participants.Length} participants");
        foreach (ChatParticipant participant in groupChat.Participants)
        {
            Debug.Log($"Participant: {participant.Username}");
        }
    }
}
catch (ApiException ex)
{
    Debug.LogError($"Failed to create group chat: {ex.Message}");
}
```

### GetMessagesAsync

Retrieves a paginated list of messages for a specific chat.

**Parameters:**
- `int chatId` (required): ID of the chat to retrieve messages for
- `int page = 1` (optional): Page number for pagination

**Returns:**
- `Task<GetMessagesResponse>`: Response containing messages for the chat

**Example:**
```csharp
try
{
    // Get the current authenticated user
    GameFuseUser currentUser = GameFuseUser.CurrentUser;
    
    // Get messages for a specific chat (first page)
    int chatId = 123;
    GetMessagesResponse messagesResponse = await currentUser.GetMessagesAsync(chatId);
    
    // Access the messages
    ChatMessage[] messages = messagesResponse.Messages;
    
    // Display the messages
    Debug.Log($"Retrieved {messages.Length} messages");
    
    foreach (ChatMessage message in messages)
    {
        Debug.Log($"Message: {message.Text}");
        Debug.Log($"From: {message.UserId}, Sent at: {message.CreatedAt}");
        Debug.Log($"Read by: {message.ReadBy.Length} users, Current user read: {message.Read}");
    }
    
    // To get older messages (next page)
    GetMessagesResponse olderMessages = await currentUser.GetMessagesAsync(chatId, 2);
}
catch (ApiException ex)
{
    Debug.LogError($"Failed to get messages: {ex.Message}");
}
```

### SendMessageAsync

Sends a new message to an existing chat.

**Parameters:**
- `int chatId` (required): ID of the chat to send the message to
- `string text` (required): Message text to send

**Returns:**
- `Task<ChatMessage>`: Response containing the sent message

**Example:**
```csharp
try
{
    // Get the current authenticated user
    GameFuseUser currentUser = GameFuseUser.CurrentUser;
    
    // Send a message to an existing chat
    int chatId = 123;
    string messageText = "Are you ready for the tournament tonight?";
    
    ChatMessage sentMessage = await currentUser.SendMessageAsync(chatId, messageText);
    
    // Access the message ID
    int messageId = sentMessage.Id;
    Debug.Log($"Message sent successfully! Message ID: {messageId}");
}
catch (ApiException ex)
{
    Debug.LogError($"Failed to send message: {ex.Message}");
}
```

### MarkMessageAsReadAsync

Marks a specific message as read by the current user.

**Parameters:**
- `int messageId` (required): ID of the message to mark as read

**Returns:**
- `Task<MessageReadResponse>`: Response confirming the message was marked as read

**Example:**
```csharp
try
{
    // Get the current authenticated user
    GameFuseUser currentUser = GameFuseUser.CurrentUser;
    
    // Mark a message as read
    int messageId = 456;
    MessageReadResponse readResponse = await currentUser.MarkMessageAsReadAsync(messageId);
    
    Debug.Log($"Message marked as read: {readResponse.Message}");
}
catch (ApiException ex)
{
    Debug.LogError($"Failed to mark message as read: {ex.Message}");
}
```

## Chat Response Objects

### Chat

The `Chat` class contains information about a chat conversation:

| Property | Type | Description |
|----------|------|-------------|
| `Id` | int | Chat identifier |
| `CreatorId` | int | User or group ID that created the chat |
| `CreatorType` | string | Type of creator (User or Group) |
| `Messages` | ChatMessage[] | Array of messages in the chat |
| `Participants` | ChatParticipant[] | Array of chat participants |

### ChatMessage

The `ChatMessage` class contains information about an individual message:

| Property | Type | Description |
|----------|------|-------------|
| `Id` | int | Message identifier |
| `Text` | string | Message content |
| `UserId` | int | ID of the user who sent the message |
| `CreatedAt` | string | Timestamp when the message was created |
| `ReadBy` | string[] | Array of user IDs who have read the message |
| `Read` | bool | Whether the current user has read the message |

### ChatParticipant

The `ChatParticipant` class extends `UserInfo` and contains information about chat participants:

| Property | Type | Description |
|----------|------|-------------|
| `IsNewUser` | bool | Whether the user is new to the chat |
| *(Plus all properties inherited from UserInfo)* | | |

### GetChatsResponse

The `GetChatsResponse` class contains the response from getting chats:

| Property | Type | Description |
|----------|------|-------------|
| `DirectChats` | Chat[] | Array of direct chats |
| `GroupChats` | Chat[] | Array of group chats |

### GetMessagesResponse

The `GetMessagesResponse` class contains the response from getting messages:

| Property | Type | Description |
|----------|------|-------------|
| `Messages` | ChatMessage[] | Array of messages |

## Common Usage Pattern

Here's an example of how to implement a basic chat system in your game:

```csharp
using UnityEngine;
using GameFuseCSharp;
using System.Linq;
using System.Collections.Generic;
using UnityEngine.UI;

public class ChatManager : MonoBehaviour
{
    // UI elements for chat list
    [SerializeField] private Transform chatListContainer;
    [SerializeField] private GameObject chatItemPrefab;
    
    // UI elements for message display
    [SerializeField] private Transform messageContainer;
    [SerializeField] private GameObject messageItemPrefab;
    [SerializeField] private InputField messageInputField;
    [SerializeField] private Button sendMessageButton;
    [SerializeField] private Text chatTitleText;
    
    // Current active chat
    private int currentChatId = -1;
    private string currentChatTitle = "";
    private Dictionary<int, List<ChatMessage>> cachedMessages = new Dictionary<int, List<ChatMessage>>();
    
    private void Start()
    {
        if (sendMessageButton != null)
        {
            sendMessageButton.onClick.AddListener(HandleSendMessage);
        }
    }
    
    // Load all chats for the current user
    public async void LoadChats()
    {
        try
        {
            GameFuseUser currentUser = GameFuseUser.CurrentUser;
            if (currentUser == null)
            {
                Debug.LogError("User not logged in");
                return;
            }
            
            // Clear existing items
            foreach (Transform child in chatListContainer)
            {
                Destroy(child.gameObject);
            }
            
            // Get all chats for the current user
            GetChatsResponse response = await currentUser.GetChatsAsync();
            
            // Create UI items for direct chats
            foreach (Chat chat in response.DirectChats)
            {
                GameObject chatItem = Instantiate(chatItemPrefab, chatListContainer);
                ChatItemUI chatUI = chatItem.GetComponent<ChatItemUI>();
                
                // Find the other participant(s) for display
                string chatName = GetChatDisplayName(chat);
                
                chatUI.SetupChat(chat.Id, chatName, chat);
                chatUI.OnChatSelected += SelectChat;
            }
            
            // Create UI items for group chats
            foreach (Chat chat in response.GroupChats)
            {
                GameObject chatItem = Instantiate(chatItemPrefab, chatListContainer);
                ChatItemUI chatUI = chatItem.GetComponent<ChatItemUI>();
                
                // Use group name for display
                string chatName = "Group: " + chat.CreatorId;  // You would get the actual group name
                
                chatUI.SetupChat(chat.Id, chatName, chat);
                chatUI.OnChatSelected += SelectChat;
            }
            
            Debug.Log($"Loaded {response.DirectChats.Length} direct chats and {response.GroupChats.Length} group chats");
        }
        catch (ApiException ex)
        {
            Debug.LogError($"Error loading chats: {ex.Message}");
        }
    }
    
    // Helper method to get a display name for a chat
    private string GetChatDisplayName(Chat chat)
    {
        if (chat.Participants == null || chat.Participants.Length == 0)
        {
            return "Empty Chat";
        }
        
        // For direct chats, show the other participant's name
        if (chat.CreatorType == "User")
        {
            // Find participants who are not the current user
            var otherParticipants = chat.Participants.Where(p => p.Id != GameFuseUser.CurrentUser.GetID());
            return string.Join(", ", otherParticipants.Select(p => p.Username));
        }
        
        // For group chats, could fetch the group name
        return "Group Chat";
    }
    
    // Select a chat to view
    public async void SelectChat(int chatId, string chatName, Chat chatData = null)
    {
        currentChatId = chatId;
        currentChatTitle = chatName;
        
        if (chatTitleText != null)
        {
            chatTitleText.text = chatName;
        }
        
        await LoadMessages(chatId);
    }
    
    // Load messages for a selected chat
    public async System.Threading.Tasks.Task LoadMessages(int chatId)
    {
        try
        {
            GameFuseUser currentUser = GameFuseUser.CurrentUser;
            if (currentUser == null)
            {
                Debug.LogError("User not logged in");
                return;
            }
            
            // Clear existing messages
            foreach (Transform child in messageContainer)
            {
                Destroy(child.gameObject);
            }
            
            // Get messages for the selected chat
            GetMessagesResponse response = await currentUser.GetMessagesAsync(chatId);
            
            // Cache the messages
            if (!cachedMessages.ContainsKey(chatId))
            {
                cachedMessages[chatId] = new List<ChatMessage>();
            }
            else
            {
                cachedMessages[chatId].Clear();
            }
            
            // Add messages to cache
            cachedMessages[chatId].AddRange(response.Messages);
            
            // Create UI items for each message
            DisplayMessages(chatId);
            
            // Mark all messages as read
            foreach (ChatMessage message in response.Messages)
            {
                if (!message.Read)
                {
                    await currentUser.MarkMessageAsReadAsync(message.Id);
                }
            }
        }
        catch (ApiException ex)
        {
            Debug.LogError($"Error loading messages: {ex.Message}");
        }
    }
    
    // Display cached messages for a chat
    private void DisplayMessages(int chatId)
    {
        if (!cachedMessages.ContainsKey(chatId) || cachedMessages[chatId].Count == 0)
        {
            return;
        }
        
        foreach (ChatMessage message in cachedMessages[chatId])
        {
            GameObject messageItem = Instantiate(messageItemPrefab, messageContainer);
            MessageItemUI messageUI = messageItem.GetComponent<MessageItemUI>();
            
            bool isFromCurrentUser = message.UserId == GameFuseUser.CurrentUser.GetID();
            messageUI.SetupMessage(message.Text, message.CreatedAt, isFromCurrentUser);
        }
        
        // Scroll to bottom (you'd need to implement this based on your UI)
        ScrollToBottom();
    }
    
    // Send a new message
    public async void HandleSendMessage()
    {
        if (currentChatId < 0)
        {
            Debug.LogWarning("No chat selected");
            return;
        }
        
        string messageText = messageInputField.text;
        if (string.IsNullOrEmpty(messageText))
        {
            return;
        }
        
        try
        {
            GameFuseUser currentUser = GameFuseUser.CurrentUser;
            if (currentUser == null)
            {
                Debug.LogError("User not logged in");
                return;
            }
            
            // Send the message
            ChatMessage sentMessage = await currentUser.SendMessageAsync(currentChatId, messageText);
            
            // Clear the input field
            messageInputField.text = "";
            
            // Add the new message to cache
            if (cachedMessages.ContainsKey(currentChatId))
            {
                cachedMessages[currentChatId].Add(sentMessage);
                
                // Create UI for the new message
                GameObject messageItem = Instantiate(messageItemPrefab, messageContainer);
                MessageItemUI messageUI = messageItem.GetComponent<MessageItemUI>();
                messageUI.SetupMessage(sentMessage.Text, sentMessage.CreatedAt, true);
                
                // Scroll to bottom
                ScrollToBottom();
            }
            
            Debug.Log($"Message sent: {sentMessage.Text}");
        }
        catch (ApiException ex)
        {
            Debug.LogError($"Error sending message: {ex.Message}");
        }
    }
    
    // Create a new direct chat
    public async void CreateDirectChat(string username)
    {
        try
        {
            GameFuseUser currentUser = GameFuseUser.CurrentUser;
            if (currentUser == null)
            {
                Debug.LogError("User not logged in");
                return;
            }
            
            // Create a direct chat
            string[] usernames = new string[] { username };
            string initialMessage = "Hello! I'd like to chat.";
            
            Chat newChat = await currentUser.CreateDirectChatAsync(usernames, initialMessage);
            
            // Refresh the chat list
            LoadChats();
            
            // Select the new chat
            SelectChat(newChat.Id, username, newChat);
            
            Debug.Log($"Created new direct chat with {username}");
        }
        catch (ApiException ex)
        {
            Debug.LogError($"Error creating direct chat: {ex.Message}");
        }
    }
    
    // Create a new group chat
    public async void CreateGroupChat(int groupId, string groupName)
    {
        try
        {
            GameFuseUser currentUser = GameFuseUser.CurrentUser;
            if (currentUser == null)
            {
                Debug.LogError("User not logged in");
                return;
            }
            
            // Create a group chat
            string initialMessage = "Hello everyone! I've created a group chat for us.";
            
            Chat newChat = await currentUser.CreateGroupChatAsync(groupId, initialMessage);
            
            // Refresh the chat list
            LoadChats();
            
            // Select the new chat
            SelectChat(newChat.Id, "Group: " + groupName, newChat);
            
            Debug.Log($"Created new group chat for group {groupName}");
        }
        catch (ApiException ex)
        {
            Debug.LogError($"Error creating group chat: {ex.Message}");
        }
    }
    
    // Helper method to scroll the message view to the bottom
    private void ScrollToBottom()
    {
        // Implement based on your UI components
        // For example, if using a ScrollRect:
        // messageScrollRect.verticalNormalizedPosition = 0f;
    }
}

// UI component for displaying chat items in a list
public class ChatItemUI : MonoBehaviour
{
    [SerializeField] private Text chatNameText;
    [SerializeField] private Text lastMessageText;
    [SerializeField] private Button chatButton;
    
    private int chatId;
    private string chatName;
    private Chat chatData;
    
    // Event for when this chat is selected
    public System.Action<int, string, Chat> OnChatSelected;
    
    private void Start()
    {
        if (chatButton != null)
        {
            chatButton.onClick.AddListener(OnChatClicked);
        }
    }
    
    public void SetupChat(int id, string name, Chat data)
    {
        chatId = id;
        chatName = name;
        chatData = data;
        
        if (chatNameText != null)
        {
            chatNameText.text = name;
        }
        
        if (lastMessageText != null && data.Messages != null && data.Messages.Length > 0)
        {
            // Show the latest message
            ChatMessage latestMessage = data.Messages[0];
            string sender = latestMessage.UserId == GameFuseUser.CurrentUser.GetID() ? "You" : "Other";
            lastMessageText.text = $"{sender}: {latestMessage.Text}";
        }
        else if (lastMessageText != null)
        {
            lastMessageText.text = "No messages yet";
        }
    }
    
    private void OnChatClicked()
    {
        OnChatSelected?.Invoke(chatId, chatName, chatData);
    }
}

// UI component for displaying message items
public class MessageItemUI : MonoBehaviour
{
    [SerializeField] private Text messageText;
    [SerializeField] private Text timestampText;
    [SerializeField] private RectTransform messageContainer;
    
    public void SetupMessage(string text, string timestamp, bool isFromCurrentUser)
    {
        if (messageText != null)
        {
            messageText.text = text;
        }
        
        if (timestampText != null && !string.IsNullOrEmpty(timestamp))
        {
            // Format the timestamp
            if (System.DateTime.TryParse(timestamp, out System.DateTime dateTime))
            {
                timestampText.text = dateTime.ToString("HH:mm");
            }
            else
            {
                timestampText.text = timestamp;
            }
        }
        
        // Align the message based on sender
        if (messageContainer != null)
        {
            // You would implement this based on your UI layout
            // For example:
            /*
            if (isFromCurrentUser)
            {
                messageContainer.anchorMin = new Vector2(1, 0);
                messageContainer.anchorMax = new Vector2(1, 0);
                messageContainer.pivot = new Vector2(1, 0);
                messageContainer.anchoredPosition = new Vector2(-10, 0);
                messageBackground.color = currentUserBubbleColor;
            }
            else
            {
                messageContainer.anchorMin = new Vector2(0, 0);
                messageContainer.anchorMax = new Vector2(0, 0);
                messageContainer.pivot = new Vector2(0, 0);
                messageContainer.anchoredPosition = new Vector2(10, 0);
                messageBackground.color = otherUserBubbleColor;
            }
            */
        }
    }
}
```

## Error Handling

All chat methods throw an `ApiException` when a request fails. Always wrap your API calls in try-catch blocks to handle errors gracefully.

Common errors include:
- User not found (when creating a direct chat)
- Chat not found (when sending messages)
- Insufficient permissions
- Network connectivity issues