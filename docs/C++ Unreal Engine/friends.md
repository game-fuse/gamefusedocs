# Friends System

The GameFuse Friends System allows you to implement social features in your game, such as sending friend requests, accepting or declining requests, and managing a friends list.

## Getting Started with Friends

To use the GameFuse Friends system, you'll need to access the `UGameFuseFriends` subsystem:

```cpp
UGameFuseFriends* FriendsSystem = GetGameInstance()->GetSubsystem<UGameFuseFriends>();
```

## Sending Friend Requests

You can send a friend request to another user by their username:

!!! example "C++ Example"
    ```cpp
    void UMyGameMode::SendFriendRequest(const FString& Username)
    {
        UGameFuseFriends* FriendsSystem = GetGameInstance()->GetSubsystem<UGameFuseFriends>();
        
        // Create typed callback
        FGFFriendRequestCallback OnSendFriendRequest;
        OnSendFriendRequest.BindLambda([this](const FGFFriendRequest& Request) {
            UE_LOG(LogTemp, Display, TEXT("Friend request sent successfully"));
            UE_LOG(LogTemp, Display, TEXT("Friendship ID: %d"), Request.FriendshipId);
            UE_LOG(LogTemp, Display, TEXT("Other User: %s"), *Request.OtherUser.Username);
        });
        
        // Send the friend request
        FriendsSystem->SendFriendRequest(Username, OnSendFriendRequest);
    }
    ```


## Accepting Friend Requests

When someone sends you a friend request, you can accept it:

!!! example "C++ Example"
    ```cpp
    void UMyGameMode::AcceptFriendRequest(int32 FriendshipId)
    {
        UGameFuseFriends* FriendsSystem = GetGameInstance()->GetSubsystem<UGameFuseFriends>();
        
        // Create typed callback
        FGFFriendActionCallback OnAcceptFriendRequest;
        OnAcceptFriendRequest.BindLambda([this, FriendshipId](bool bSuccess) {
            if (bSuccess)
            {
                UE_LOG(LogTemp, Display, TEXT("Friend request accepted successfully"));
            }
            else
            {
                UE_LOG(LogTemp, Error, TEXT("Failed to accept friend request"));
            }
        });
        
        // Accept the friend request
        FriendsSystem->AcceptFriendRequest(FriendshipId, OnAcceptFriendRequest);
    }
    ```


## Declining Friend Requests

You can also decline a friend request:

!!! example "C++ Example"
    ```cpp
    void UMyGameMode::DeclineFriendRequest(int32 FriendshipId)
    {
        UGameFuseFriends* FriendsSystem = GetGameInstance()->GetSubsystem<UGameFuseFriends>();
        
        // Create typed callback
        FGFFriendActionCallback OnDeclineFriendRequest;
        OnDeclineFriendRequest.BindLambda([this, FriendshipId](bool bSuccess) {
            if (bSuccess)
            {
                UE_LOG(LogTemp, Display, TEXT("Friend request declined successfully"));
            }
            else
            {
                UE_LOG(LogTemp, Error, TEXT("Failed to decline friend request"));
            }
        });
        
        // Decline the friend request
        FriendsSystem->DeclineFriendRequest(FriendshipId, OnDeclineFriendRequest);
    }
    ```

## Canceling Friend Requests

If you've sent a friend request and want to cancel it:

!!! example "C++ Example"
    ```cpp
    void UMyGameMode::CancelFriendRequest(int32 FriendshipId)
    {
        UGameFuseFriends* FriendsSystem = GetGameInstance()->GetSubsystem<UGameFuseFriends>();
        
        // Create typed callback
        FGFFriendActionCallback OnCancelFriendRequest;
        OnCancelFriendRequest.BindLambda([this, FriendshipId](bool bSuccess) {
            if (bSuccess)
            {
                UE_LOG(LogTemp, Display, TEXT("Friend request canceled successfully"));
            }
            else
            {
                UE_LOG(LogTemp, Error, TEXT("Failed to cancel friend request"));
            }
        });
        
        // Cancel the friend request
        FriendsSystem->CancelFriendRequest(FriendshipId, OnCancelFriendRequest);
    }
    ```

## Unfriending Players

You can remove a user from your friends list:

!!! example "C++ Example"
    ```cpp
    void UMyGameMode::UnfriendPlayer(int32 UserId)
    {
        UGameFuseFriends* FriendsSystem = GetGameInstance()->GetSubsystem<UGameFuseFriends>();
        
        // Create typed callback
        FGFFriendActionCallback OnUnfriendPlayer;
        OnUnfriendPlayer.BindLambda([this, UserId](bool bSuccess) {
            if (bSuccess)
            {
                UE_LOG(LogTemp, Display, TEXT("Player unfriended successfully"));
            }
            else
            {
                UE_LOG(LogTemp, Error, TEXT("Failed to unfriend player"));
            }
        });
        
        // Unfriend the player
        FriendsSystem->UnfriendPlayer(UserId, OnUnfriendPlayer);
    }
    ```

## Fetching Friendship Data

You can fetch all friendship data for the current user:

!!! example "C++ Example"
    ```cpp
    void UMyGameMode::FetchFriendshipData()
    {
        UGameFuseFriends* FriendsSystem = GetGameInstance()->GetSubsystem<UGameFuseFriends>();
        
        // Create typed callback
        FGFFriendsCallback OnFetchFriendshipData;
        OnFetchFriendshipData.BindLambda([this](const TArray<FGFUserData>& FriendsList) {
            UE_LOG(LogTemp, Display, TEXT("Fetched %d friends"), FriendsList.Num());
            
            for (const FGFUserData& Friend : FriendsList)
            {
                UE_LOG(LogTemp, Display, TEXT("Friend: %s (ID: %d)"), *Friend.Username, Friend.Id);
            }
        });
        
        // Fetch friendship data
        FriendsSystem->FetchFriendshipData(OnFetchFriendshipData);
    }
    ```

## Fetching Friends Lists

### Fetching Current User's Friends List

!!! example "C++ Example"
    ```cpp
    void UMyGameMode::FetchMyFriendsList()
    {
        UGameFuseFriends* FriendsSystem = GetGameInstance()->GetSubsystem<UGameFuseFriends>();
        
        // Create typed callback
        FGFFriendsCallback OnFetchFriendsList;
        OnFetchFriendsList.BindLambda([this](const TArray<FGFUserData>& Friends) {
            UE_LOG(LogTemp, Display, TEXT("Fetched %d friends"), Friends.Num());
            
            for (const FGFUserData& Friend : Friends)
            {
                UE_LOG(LogTemp, Display, TEXT("Friend: %s (ID: %d)"), *Friend.Username, Friend.Id);
            }
        });
        
        // Fetch friends list
        FriendsSystem->FetchMyFriendsList(OnFetchFriendsList);
    }
    ```

### Fetching Other Users' Friends Lists

!!! example "C++ Example"
    ```cpp
    void UMyGameMode::FetchUserFriendsList(int32 UserId)
    {
        UGameFuseFriends* FriendsSystem = GetGameInstance()->GetSubsystem<UGameFuseFriends>();
        
        // Create typed callback
        FGFFriendsCallback OnFetchUserFriendsList;
        OnFetchUserFriendsList.BindLambda([this](const TArray<FGFUserData>& Friends) {
            UE_LOG(LogTemp, Display, TEXT("Fetched %d friends for user"), Friends.Num());
            
            for (const FGFUserData& Friend : Friends)
            {
                UE_LOG(LogTemp, Display, TEXT("Friend: %s (ID: %d)"), *Friend.Username, Friend.Id);
            }
        });
        
        // Fetch user's friends list
        FriendsSystem->FetchUserFriendsList(UserId, OnFetchUserFriendsList);
    }
    ```

## Fetching Friend Requests

### Fetching Outgoing Friend Requests

!!! example "C++ Example"
    ```cpp
    void UMyGameMode::FetchOutgoingFriendRequests()
    {
        UGameFuseFriends* FriendsSystem = GetGameInstance()->GetSubsystem<UGameFuseFriends>();
        
        // Create typed callback
        FGFFriendRequestsCallback OnFetchOutgoingRequests;
        OnFetchOutgoingRequests.BindLambda([this](const TArray<FGFFriendRequest>& Requests) {
            UE_LOG(LogTemp, Display, TEXT("Fetched %d outgoing friend requests"), Requests.Num());
            
            for (const FGFFriendRequest& Request : Requests)
            {
                UE_LOG(LogTemp, Display, TEXT("Outgoing request to: %s (ID: %d)"), 
                    *Request.OtherUser.Username, Request.FriendshipId);
            }
        });
        
        // Fetch outgoing friend requests
        FriendsSystem->FetchOutgoingFriendRequests(OnFetchOutgoingRequests);
    }
    ```

### Fetching Incoming Friend Requests

!!! example "C++ Example"
    ```cpp
    void UMyGameMode::FetchIncomingFriendRequests()
    {
        UGameFuseFriends* FriendsSystem = GetGameInstance()->GetSubsystem<UGameFuseFriends>();
        
        // Create typed callback
        FGFFriendRequestsCallback OnFetchIncomingRequests;
        OnFetchIncomingRequests.BindLambda([this](const TArray<FGFFriendRequest>& Requests) {
            UE_LOG(LogTemp, Display, TEXT("Fetched %d incoming friend requests"), Requests.Num());
            
            for (const FGFFriendRequest& Request : Requests)
            {
                UE_LOG(LogTemp, Display, TEXT("Incoming request from: %s (ID: %d)"), 
                    *Request.OtherUser.Username, Request.FriendshipId);
            }
        });
        
        // Fetch incoming friend requests
        FriendsSystem->FetchIncomingFriendRequests(OnFetchIncomingRequests);
    }
    ```

## Function Parameters

### Friend Request Operations

| Parameter | Type | Description |
|-----------|------|-------------|
| `Username` | `FString` | The username to send friend request to |
| `FriendshipId` | `int32` | The ID of the friendship request |
| `UserId` | `int32` | The ID of the user to unfriend |
| `Callback` | Various callback types | Callback function to handle the response |

### Fetching Friends Lists

| Parameter | Type | Description |
|-----------|------|-------------|
| `UserId` | `int32` | The user ID to fetch friends for (optional, for user-specific fetch) |
| `Callback` | `FGFFriendsCallback` / `FBP_GFApiCallback` | Callback function to handle the response |

## Function Return Values

### Friend Request Operations

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK - Operation successful |
| `400`            | Bad request - Invalid parameters |
| `401`            | Unauthorized - User not signed in |
| `404`            | User not found |
| `409`            | Friend request already exists |
| `500`            | Unknown server error |

### Fetching Friends Lists

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK - Friends list fetched successfully |
| `401`            | Unauthorized - User not signed in |
| `404`            | User not found (for user-specific fetch) |
| `500`            | Unknown server error |

## Cached Data Access

You can access cached friends data without making API calls:

```cpp
UGameFuseFriends* FriendsSystem = GetGameInstance()->GetSubsystem<UGameFuseFriends>();

// Get cached friends list
const TArray<FGFUserData>& FriendsList = FriendsSystem->GetFriendsList();

// Get cached outgoing requests
const TArray<FGFFriendRequest>& OutgoingRequests = FriendsSystem->GetOutgoingRequests();

// Get cached incoming requests
const TArray<FGFFriendRequest>& IncomingRequests = FriendsSystem->GetIncomingRequests();
```