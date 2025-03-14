# Chat System

The GameFuse Chat System allows you to implement real-time chat functionality in your game. This includes creating chats, sending messages, and managing message read status.

## Getting Started with Chat

To use the GameFuse Chat system, you'll need to access the `UGameFuseChat` subsystem:

```cpp
UGameFuseChat* ChatSystem = GetGameInstance()->GetSubsystem<UGameFuseChat>();
```

## Creating a Chat

You can create a new chat with one or more participants:

```cpp
void UMyGameMode::CreateChat()
{
    UGameFuseChat* ChatSystem = GetGameInstance()->GetSubsystem<UGameFuseChat>();
    
    // Create an array of participant usernames
    TArray<FString> Usernames;
    Usernames.Add("OtherPlayerUsername"); // Add usernames of participants
    
    // Initial message to send
    FString InitialMessage = "Hello! Welcome to the chat.";
    
    // Create typed callback
    FGFChatCallback OnCreateChat;
    OnCreateChat.BindLambda([this](const FGFChat& Chat) {
        // Use Chat data (ID, participants, messages, etc.)
    });
    
    // Create the chat
    ChatSystem->CreateChat(Usernames, InitialMessage, OnCreateChat);
}

void UMyGameMode::OnChatCreated(bool bSuccess, const FString& Response)
{
    if (bSuccess)
    {
        // Parse Response JSON and use chat data
    }
    else
    {
        // Handle error
    }
}
```

For Blueprint usage, you can use the BP_ prefixed version with the generic callback:

```cpp
void UMyGameMode::CreateChatBP()
{
    UGameFuseChat* ChatSystem = GetGameInstance()->GetSubsystem<UGameFuseChat>();
    
    // Create an array of participant usernames
    TArray<FString> Usernames;
    Usernames.Add("OtherPlayerUsername"); // Add usernames of participants
    
    // Initial message to send
    FString InitialMessage = "Hello! Welcome to the chat.";
    
    // Create callback
    FBP_GFApiCallback Callback;
    Callback.BindDynamic(this, &UMyGameMode::OnChatCreated);
    
    // Create the chat
    ChatSystem->BP_CreateChat(Usernames, InitialMessage, Callback);
}

void UMyGameMode::OnChatCreated(bool bSuccess, const FString& Response)
{
    if (bSuccess)
    {
        UE_LOG(LogTemp, Display, TEXT("Chat created successfully"));
        UE_LOG(LogTemp, Display, TEXT("Response: %s"), *Response);
        
        // Access the created chat
        UGameFuseChat* ChatSystem = GetGameInstance()->GetSubsystem<UGameFuseChat>();
        const TArray<FGFChat>& AllChats = ChatSystem->GetAllChats();
        
        // The newly created chat should be in the list
        if (AllChats.Num() > 0)
        {
            UE_LOG(LogTemp, Display, TEXT("Chat ID: %d"), AllChats.Last().Id);
        }
    }
    else
    {
        UE_LOG(LogTemp, Error, TEXT("Failed to create chat"));
        UE_LOG(LogTemp, Error, TEXT("Error: %s"), *Response);
    }
}
```

## Sending Messages

Once you have a chat, you can send messages to it:

```cpp
void UMyGameMode::SendMessage(int32 ChatId, const FString& Message)
{
    UGameFuseChat* ChatSystem = GetGameInstance()->GetSubsystem<UGameFuseChat>();
    
    // Create typed callback
    FGFMessageCallback OnSendMessage;
    OnSendMessage.BindLambda([this](const FGFMessage& SentMessage) {
        // Use SentMessage data (ID, text, chatId, etc.)
    });
    
    // Send the message
    ChatSystem->SendMessage(ChatId, Message, OnSendMessage);
}
```

For Blueprint usage:

```cpp
void UMyGameMode::SendMessageBP(int32 ChatId, const FString& Message)
{
    UGameFuseChat* ChatSystem = GetGameInstance()->GetSubsystem<UGameFuseChat>();
    
    // Create callback
    FBP_GFApiCallback Callback;
    Callback.BindDynamic(this, &UMyGameMode::OnMessageSent);
    
    // Send the message
    ChatSystem->BP_SendMessage(ChatId, Message, Callback);
}

void UMyGameMode::OnMessageSent(bool bSuccess, const FString& Response)
{
    if (bSuccess)
    {
        // Handle successful message send
    }
    else
    {
        // Handle error
    }
}
```

## Marking Messages as Read

You can mark messages as read to keep track of which messages have been seen:

```cpp
void UMyGameMode::MarkMessageAsRead(int32 MessageId)
{
    UGameFuseChat* ChatSystem = GetGameInstance()->GetSubsystem<UGameFuseChat>();
    
    // Create typed callback
    FGFMessageActionCallback OnMarkMessageAsRead;
    OnMarkMessageAsRead.BindLambda([this, MessageId](bool bSuccess) {
        // Handle success or failure of marking message as read
    });
    
    // Mark the message as read
    ChatSystem->MarkMessageAsRead(MessageId, OnMarkMessageAsRead);
}
```

For Blueprint usage:

```cpp
void UMyGameMode::MarkMessageAsReadBP(int32 MessageId)
{
    UGameFuseChat* ChatSystem = GetGameInstance()->GetSubsystem<UGameFuseChat>();
    
    // Create callback
    FBP_GFApiCallback Callback;
    Callback.BindDynamic(this, &UMyGameMode::OnMessageMarkedAsRead);
    
    // Mark the message as read
    ChatSystem->BP_MarkMessageAsRead(MessageId, Callback);
}

void UMyGameMode::OnMessageMarkedAsRead(bool bSuccess, const FString& Response)
{
    if (bSuccess)
    {
        // Handle successful mark as read
    }
    else
    {
        // Handle error
    }
}
```

## Fetching Chats and Messages

You can fetch all chats for the current user:

```cpp
void UMyGameMode::FetchAllChats()
{
    UGameFuseChat* ChatSystem = GetGameInstance()->GetSubsystem<UGameFuseChat>();
    
    // Create typed callback
    FGFChatsCallback OnFetchAllChats;
    OnFetchAllChats.BindLambda([this](const TArray<FGFChat>& Chats) {
        // Process array of chats for display
    });
    
    // Fetch all chats (page 1)
    ChatSystem->FetchAllChats(1, OnFetchAllChats);
}
```

For Blueprint usage:

```cpp
void UMyGameMode::FetchAllChatsBP()
{
    UGameFuseChat* ChatSystem = GetGameInstance()->GetSubsystem<UGameFuseChat>();
    
    // Create callback
    FBP_GFApiCallback Callback;
    Callback.BindDynamic(this, &UMyGameMode::OnChatsReceived);
    
    // Fetch all chats (page 1)
    ChatSystem->BP_FetchAllChats(1, Callback);
}

void UMyGameMode::OnChatsReceived(bool bSuccess, const FString& Response)
{
    if (bSuccess)
    {
        // Process chats from ChatSystem->GetAllChats()
    }
    else
    {
        // Handle error
    }
}
```

You can also fetch messages for a specific chat:

```cpp
void UMyGameMode::FetchMessages(int32 ChatId)
{
    UGameFuseChat* ChatSystem = GetGameInstance()->GetSubsystem<UGameFuseChat>();
    
    // Create typed callback
    FGFMessagesCallback OnFetchMessages;
    OnFetchMessages.BindLambda([this, ChatId](const TArray<FGFMessage>& Messages) {
        // Process array of messages for display
    });
    
    // Fetch messages (page 1)
    ChatSystem->FetchMessages(ChatId, 1, OnFetchMessages);
}
```

For Blueprint usage:

```cpp
void UMyGameMode::FetchMessagesBP(int32 ChatId)
{
    UGameFuseChat* ChatSystem = GetGameInstance()->GetSubsystem<UGameFuseChat>();
    
    // Create callback
    FBP_GFApiCallback Callback;
    Callback.BindDynamic(this, &UMyGameMode::OnMessagesReceived);
    
    // Fetch messages (page 1)
    ChatSystem->BP_FetchMessages(ChatId, 1, Callback);
}

void UMyGameMode::OnMessagesReceived(bool bSuccess, const FString& Response)
{
    if (bSuccess)
    {
        // Process messages from ChatSystem->GetChatMessages()
    }
    else
    {
        // Handle error
    }
}
```

## Clearing Chat Data

You can clear the cached chat data when it's no longer needed:

```cpp
UGameFuseChat* ChatSystem = GetGameInstance()->GetSubsystem<UGameFuseChat>();
ChatSystem->ClearChatData();
```

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