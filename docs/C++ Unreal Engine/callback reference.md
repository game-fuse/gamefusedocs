# GameFuse Callback Reference

This page serves as a reference for all callback types used in the GameFuse systems. It includes information on the different callback types, their signatures, and examples of how to bind to them using various methods.

!!! note
    These callbacks are the main benefit of using C++ with GameFuse. By using them, you no longer need to retrieve the data from the subsystem, as it is called as a parameter to your function.

## New Delegate System (V2.9+)

GameFuse V2.9 introduced a new delegate system that replaces the old latent nodes. All API calls now use delegates for callbacks, providing better performance and more flexibility.

### Main Delegate Types

#### FBP_GFApiCallback (Blueprint)
```cpp
DECLARE_DYNAMIC_DELEGATE_OneParam(FBP_GFApiCallback, FGFAPIResponse, ResponseData);
```
This is the main callback delegate for Blueprint usage. It receives an `FGFAPIResponse` struct containing the success status, response data, request ID, and response code.

#### FGFApiCallback (C++)
```cpp
DECLARE_MULTICAST_DELEGATE_OneParam(FGFApiCallback, FGFAPIResponse);
```
This is the C++ version of the API callback delegate. It's a multicast delegate that can be bound multiple times.

## Binding to Callbacks

There are several ways to bind to callbacks in Unreal Engine. 
[Credit to BenUI for the information.](https://unreal-garden.com/tutorials/delegates-advanced/)
Here are the most common methods:

### Using BindLambda

The `BindLambda` method is useful for simple, anonymous functions:

```cpp
FGFApiCallback OnGameSetup;
OnGameSetup.BindLambda([this](const FGFAPIResponse& Response) {
    if (Response.bSuccess) {
        // Handle successful game setup
        UE_LOG(LogTemp, Log, TEXT("Game setup successful: %s"), *Response.ResponseStr);
    } else {
        // Handle error
        UE_LOG(LogTemp, Error, TEXT("Game setup failed: %s"), *Response.ResponseStr);
    }
});

GameFuseManager->SetUpGame(GameId, Token, OnGameSetup);
```

### Using BindUObject

The `BindUObject` method requires that the target be a UObject:

```cpp
FGFApiCallback OnGameSetup;
OnGameSetup.BindUObject(this, &ThisClass::HandleGameSetup);

GameFuseManager->SetUpGame(GameId, Token, OnGameSetup);

void HandleGameSetup(const FGFAPIResponse& Response) {
    if (Response.bSuccess) {
        // Handle successful game setup
    } else {
        // Handle error
    }
}
```

### Using BindDynamic (for Blueprint accessible functions)

The `BindDynamic` method is used for Blueprint-exposed callbacks:

```cpp
FBP_GFApiCallback Callback;
Callback.BindDynamic(this, &ThisClass::HandleGameSetup);

GameFuseManager->BP_SetUpGame(GameId, Token, Callback);

UFUNCTION(BlueprintCallable)
void HandleGameSetup(const FGFAPIResponse& Response) {
    if (Response.bSuccess) {
        // Handle successful game setup
    } else {
        // Handle error
    }
}
```

## Typed Callback Types

For C++ usage, GameFuse provides typed callbacks that return specific data structures instead of the generic `FGFAPIResponse`. These provide better type safety and easier data access.

### User System Callbacks

| Callback Type | Signature | Description |
|---------------|-----------|-------------|
| `FGFUserDataCallback` | `void(bool, const FGFUserData&)` | Called when user data is fetched or updated |
| `FGFStoreItemsCallback` | `void(bool, const TArray<FGFStoreItem>&)` | Called when store items are fetched |
| `FGFLeaderboardEntriesCallback` | `void(bool, const TArray<FGFLeaderboardEntry>&)` | Called when leaderboard entries are fetched |
| `FGFAttributesCallback` | `void(bool, const FGFAttributeList&)` | Called when user attributes are fetched or updated |

### Friends System Callbacks

| Callback Type | Signature | Description |
|---------------|-----------|-------------|
| `FGFFriendRequestCallback` | `void(const FGFFriendRequest&)` | Called when a friend request is sent |
| `FGFFriendRequestsCallback` | `void(const TArray<FGFFriendRequest>&)` | Called when friend requests are fetched |
| `FGFFriendsCallback` | `void(const TArray<FGFUserData>&)` | Called when friends list is fetched |
| `FGFFriendActionCallback` | `void(bool)` | Called when a friend action (accept/decline/cancel/unfriend) is performed |

### Groups System Callbacks

| Callback Type | Signature | Description |
|---------------|-----------|-------------|
| `FGFGroupCallback` | `void(const FGFGroup&)` | Called when a group is created or fetched |
| `FGFGroupListCallback` | `void(const TArray<FGFGroup>&)` | Called when multiple groups are fetched |
| `FGFGroupConnectionCallback` | `void(const FGFGroupConnection&)` | Called when a group connection is created |
| `FGFGroupAttributeCallback` | `void(const TArray<FGFGroupAttribute>&)` | Called when group attributes are fetched or updated |
| `FGFGroupActionCallback` | `void(bool)` | Called when a group action is performed |

### Chat System Callbacks

| Callback Type | Signature | Description |
|---------------|-----------|-------------|
| `FGFChatCallback` | `void(const FGFChat&)` | Called when a chat is created or fetched |
| `FGFChatListCallback` | `void(const TArray<FGFChat>&)` | Called when multiple chats are fetched |
| `FGFMessageCallback` | `void(const FGFMessage&)` | Called when a message is sent |
| `FGFMessageListCallback` | `void(const TArray<FGFMessage>&)` | Called when messages are fetched |

### Rounds System Callbacks

| Callback Type | Signature | Description |
|---------------|-----------|-------------|
| `FGFGameRoundCallback` | `void(const FGFGameRound&)` | Called when a game round is created, fetched, or updated |
| `FGFGameRoundListCallback` | `void(const TArray<FGFGameRound>&)` | Called when multiple game rounds are fetched |
| `FGFGameRoundActionCallback` | `void(bool)` | Called when a game round action is performed |

### Internal Callbacks

| Callback Type | Signature | Description |
|---------------|-----------|-------------|
| `FGFInternalSuccessCallback` | `void(bool)` | Internal callback for success/failure operations |
| `FGFSuccessCallback` | `void(bool)` | Simple success/failure callback |

## Example Usage

### Using Typed Callbacks (C++)

```cpp
// User sign in with typed callback
FGFUserDataCallback OnSignIn;
OnSignIn.BindLambda([this](bool bSuccess, const FGFUserData& UserData) {
    if (bSuccess) {
        UE_LOG(LogTemp, Log, TEXT("Signed in as: %s"), *UserData.Username);
        // User data is directly available
    } else {
        UE_LOG(LogTemp, Error, TEXT("Sign in failed"));
    }
});

GameFuseUser->SignIn(Email, Password, OnSignIn);
```

### Using Generic Callbacks (Blueprint Compatible)

```cpp
// User sign in with generic callback
FBP_GFApiCallback Callback;
Callback.BindDynamic(this, &ThisClass::OnSignInComplete);

GameFuseUser->BP_SignIn(Email, Password, Callback);

UFUNCTION(BlueprintCallable)
void OnSignInComplete(const FGFAPIResponse& Response) {
    if (Response.bSuccess) {
        // Parse response data manually or use subsystem getters
        const FGFUserData& UserData = GameFuseUser->GetCurrentUserData();
        UE_LOG(LogTemp, Log, TEXT("Signed in as: %s"), *UserData.Username);
    } else {
        UE_LOG(LogTemp, Error, TEXT("Sign in failed: %s"), *Response.ResponseStr);
    }
}
```

## Additional Resources

For more information on delegates in Unreal Engine, check out these resources:

- [Advanced Delegates in C++](https://benui.ca/unreal/delegates-advanced/) - A comprehensive guide to delegates in Unreal Engine
- [Unreal Engine Documentation on Delegates](https://docs.unrealengine.com/en-US/ProgrammingAndScripting/ProgrammingWithCPP/UnrealArchitecture/Delegates/index.html)
