# Friends System

The GameFuse Friends System allows you to implement social features in your game, such as sending friend requests, accepting or declining requests, and managing a friends list.

## Getting Started with Friends

To use the GameFuse Friends system, you'll need to access the `UGameFuseFriends` subsystem:

```cpp
UGameFuseFriends* FriendsSystem = GetGameInstance()->GetSubsystem<UGameFuseFriends>();
```

## Sending Friend Requests

You can send a friend request to another user by their username:

```cpp
void UMyGameMode::SendFriendRequest(const FString& Username)
{
    UGameFuseFriends* FriendsSystem = GetGameInstance()->GetSubsystem<UGameFuseFriends>();
    
    // Create typed callback
    FGFFriendRequestCallback OnSendFriendRequest;
    OnSendFriendRequest.BindLambda([this](const FGFFriendRequest& Request) {
        // Use Request data (friendshipId, otherUser, etc.)
    });
    
    // Send the friend request
    FriendsSystem->SendFriendRequest(Username, OnSendFriendRequest);
}
```

For Blueprint usage, you can use the BP_ prefixed version with the generic callback:

```cpp
void UMyGameMode::SendFriendRequestBP(const FString& Username)
{
    UGameFuseFriends* FriendsSystem = GetGameInstance()->GetSubsystem<UGameFuseFriends>();
    
    // Create callback
    FBP_GFApiCallback Callback;
    Callback.BindDynamic(this, &UMyGameMode::OnFriendRequestSent);
    
    // Send the friend request
    FriendsSystem->BP_SendFriendRequest(Username, Callback);
}

void UMyGameMode::OnFriendRequestSent(bool bSuccess, const FString& Response)
{
    if (bSuccess)
    {
        // Parse Response JSON and use request data
    }
    else
    {
        // Handle error
    }
}
```

## Accepting Friend Requests

When someone sends you a friend request, you can accept it:

```cpp
void UMyGameMode::AcceptFriendRequest(int32 FriendshipId)
{
    UGameFuseFriends* FriendsSystem = GetGameInstance()->GetSubsystem<UGameFuseFriends>();
    
    // Create typed callback
    FGFFriendActionCallback OnAcceptFriendRequest;
    OnAcceptFriendRequest.BindLambda([this, FriendshipId](bool bSuccess) {
        // Handle success or failure of accepting friend request
    });
    
    // Accept the friend request
    FriendsSystem->AcceptFriendRequest(FriendshipId, OnAcceptFriendRequest);
}
```

## Declining Friend Requests

You can also decline a friend request:

```cpp
void UMyGameMode::DeclineFriendRequest(int32 FriendshipId)
{
    UGameFuseFriends* FriendsSystem = GetGameInstance()->GetSubsystem<UGameFuseFriends>();
    
    // Create typed callback
    FGFFriendActionCallback OnDeclineFriendRequest;
    OnDeclineFriendRequest.BindLambda([this, FriendshipId](bool bSuccess) {
        // Handle success or failure of declining friend request
    });
    
    // Decline the friend request
    FriendsSystem->DeclineFriendRequest(FriendshipId, OnDeclineFriendRequest);
}
```

## Canceling Friend Requests

If you've sent a friend request and want to cancel it:

```cpp
void UMyGameMode::CancelFriendRequest(int32 FriendshipId)
{
    UGameFuseFriends* FriendsSystem = GetGameInstance()->GetSubsystem<UGameFuseFriends>();
    
    // Create typed callback
    FGFFriendActionCallback OnCancelFriendRequest;
    OnCancelFriendRequest.BindLambda([this, FriendshipId](bool bSuccess) {
        // Handle success or failure of canceling friend request
    });
    
    // Cancel the friend request
    FriendsSystem->CancelFriendRequest(FriendshipId, OnCancelFriendRequest);
}
```

## Unfriending Players

You can remove a user from your friends list:

```cpp
void UMyGameMode::UnfriendPlayer(int32 UserId)
{
    UGameFuseFriends* FriendsSystem = GetGameInstance()->GetSubsystem<UGameFuseFriends>();
    
    // Create typed callback
    FGFFriendActionCallback OnUnfriendPlayer;
    OnUnfriendPlayer.BindLambda([this, UserId](bool bSuccess) {
        // Handle success or failure of unfriending player
    });
    
    // Unfriend the player
    FriendsSystem->UnfriendPlayer(UserId, OnUnfriendPlayer);
}
```

## Fetching Friendship Data

You can fetch all friendship data for the current user:

```cpp
void UMyGameMode::FetchFriendshipData()
{
    UGameFuseFriends* FriendsSystem = GetGameInstance()->GetSubsystem<UGameFuseFriends>();
    
    // Create typed callback
    FGFFriendshipDataCallback OnFetchFriendshipData;
    OnFetchFriendshipData.BindLambda([this](const TArray<FGFUserData>& FriendsList, 
                                   const TArray<FGFFriendRequest>& OutgoingRequests, 
                                   const TArray<FGFFriendRequest>& IncomingRequests) {
        // Use FriendsList, OutgoingRequests, and IncomingRequests data
    });
    
    // Fetch friendship data
    FriendsSystem->FetchFriendshipData(OnFetchFriendshipData);
}
```

## Fetching Friends List

You can fetch just the friends list:

```cpp
void UMyGameMode::FetchFriendsList()
{
    UGameFuseFriends* FriendsSystem = GetGameInstance()->GetSubsystem<UGameFuseFriends>();
    
    // Create typed callback
    FGFFriendsCallback OnFetchFriendsList;
    OnFetchFriendsList.BindLambda([this](const TArray<FGFUserData>& Friends) {
        // Process array of friends for display
    });
    
    // Fetch friends list
    FriendsSystem->FetchFriendsList(OnFetchFriendsList);
}
```

## Fetching Friend Requests

You can fetch outgoing friend requests:

```cpp
void UMyGameMode::FetchOutgoingFriendRequests()
{
    UGameFuseFriends* FriendsSystem = GetGameInstance()->GetSubsystem<UGameFuseFriends>();
    
    // Create typed callback
    FGFFriendRequestsCallback OnFetchOutgoingRequests;
    OnFetchOutgoingRequests.BindLambda([this](const TArray<FGFFriendRequest>& Requests) {
        // Process array of outgoing requests for display
    });
    
    // Fetch outgoing friend requests
    FriendsSystem->FetchOutgoingFriendRequests(OnFetchOutgoingRequests);
}
```

And incoming friend requests:

```cpp
void UMyGameMode::FetchIncomingFriendRequests()
{
    UGameFuseFriends* FriendsSystem = GetGameInstance()->GetSubsystem<UGameFuseFriends>();
    
    // Create typed callback
    FGFFriendRequestsCallback OnFetchIncomingRequests;
    OnFetchIncomingRequests.BindLambda([this](const TArray<FGFFriendRequest>& Requests) {
        // Process array of incoming requests for display
    });
    
    // Fetch incoming friend requests
    FriendsSystem->FetchIncomingFriendRequests(OnFetchIncomingRequests);
}
```