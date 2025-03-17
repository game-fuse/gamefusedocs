# GameFuse Callback Reference

This page serves as a reference for all callback types used in the GameFuse systems. It includes information on the different callback types, their signatures, and examples of how to bind to them using various methods.

!!! note
    These callbacks are the main benefit of using C++ with GameFuse. By using them, you no longer need to retrieve the data from the subsystem, as it is called as a parameter to your function.


## Binding to Callbacks

There are several ways to bind to callbacks in Unreal Engine. 
[Credit to BenUI for the information.](https://benui.ca/unreal/delegates-advanced/#3-subscribe-to-a-delegate)
Here are the most common methods:

### Using BindLambda

The `BindLambda` method is useful for simple, anonymous functions:

```cpp
FGFFriendRequestCallback OnSendFriendRequest;
OnSendFriendRequest.BindLambda([this](const FGFFriendRequest& Request) {
    // use FriendRequest data ...
});

FriendsSystem->SendFriendRequest(Username, OnSendFriendRequest);
```

### Using BindUObject

The `BindUObject` method requires that the target be a UObject:

```cpp
FGFFriendRequestCallback OnSendFriendRequest;
OnSendFriendRequest.BindUObject(this, &ThisClass::HandleFriendRequestSent);

FriendsSystem->SendFriendRequest(Username, OnSendFriendRequest);

void HandleFriendRequestSent(const FGFFriendRequest& Request) {
    // use FriendRequest data ...
}
```

### Using BindDynamic (for Blueprint accesible functions)

The `BindDynamic` method is used for Blueprint-exposed callbacks:

```cpp
FGFFriendRequestCallback OnSendFriendRequest;
OnSendFriendRequest.BindUObject(this, &ThisClass::HandleFriendRequestSent);


FriendsSystem->BP_SendFriendRequest(Username, Callback);

UFUNCTION(BlueprintCallable)
void HandleFriendRequestSent(const FGFFriendRequest& Request) {
    // use FriendRequest data ...
}

```

## Callback Types Overview

GameFuse uses a variety of callback types for different operations. These callbacks follow a consistent naming pattern:

- `FGF[Feature]Callback`: For callbacks that return a single item
- `FGF[Feature]sCallback`: For callbacks that return an array of items
- `FGF[Feature]ActionCallback`: For callbacks that return a success/failure boolean

For Blueprint compatibility, there's also a generic callback type:
- `FBP_GFApiCallback`: Generic callback for Blueprint usage

## Common Callback Types

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
| `FGFGroupsCallback` | `void(const TArray<FGFGroup>&)` | Called when multiple groups are fetched |
| `FGFGroupActionCallback` | `void(bool)` | Called when a group action is performed |

### Chat System Callbacks

| Callback Type | Signature | Description |
|---------------|-----------|-------------|
| `FGFChatCallback` | `void(const FGFChat&)` | Called when a chat is created or fetched |
| `FGFChatsCallback` | `void(const TArray<FGFChat>&)` | Called when multiple chats are fetched |
| `FGFMessageCallback` | `void(const FGFMessage&)` | Called when a message is sent |
| `FGFMessagesCallback` | `void(const TArray<FGFMessage>&)` | Called when messages are fetched |
| `FGFMessageActionCallback` | `void(bool)` | Called when a message action is performed |

### Rounds System Callbacks

| Callback Type | Signature | Description |
|---------------|-----------|-------------|
| `FGFGameRoundCallback` | `void(const FGFGameRound&)` | Called when a game round is created, fetched, or updated |
| `FGFGameRoundsCallback` | `void(const TArray<FGFGameRound>&)` | Called when multiple game rounds are fetched |
| `FGFGameRoundActionCallback` | `void(bool)` | Called when a game round action is performed |



## Additional Resources

For more information on delegates in Unreal Engine, check out these resources:

- [Advanced Delegates in C++](https://benui.ca/unreal/delegates-advanced/) - A comprehensive guide to delegates in Unreal Engine
- [Unreal Engine Documentation on Delegates](https://docs.unrealengine.com/en-US/ProgrammingAndScripting/ProgrammingWithCPP/UnrealArchitecture/Delegates/index.html)
