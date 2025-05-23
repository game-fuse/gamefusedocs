# Friendship System

GameFuse provides a comprehensive friendship system that allows players to connect with each other. The friendship functionality is accessible through the authenticated `GameFuseUser.CurrentUser` instance.

## Getting Started

All friendship methods require user authentication. Ensure you have a signed-in user before calling any friendship methods:

!!! example
    ```csharp
    if (GameFuseUser.CurrentUser == null)
    {
        Debug.LogError("User must be authenticated first");
        return;
    }
    ```

## Sending Friend Requests

Use `SendFriendRequestAsync` to send a friend request to another player by username:

!!! example
    ```csharp
    async void SendFriendRequest()
    {
        try
        {
            var response = await GameFuseUser.CurrentUser.SendFriendRequestAsync("playerUsername");
            Debug.Log($"Friend request sent! Friendship ID: {response.FriendshipId}");
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Failed to send friend request: {ex.Message}");
        }
    }
    ```

## Managing Friend Requests

### Accept Friend Requests

Use `AcceptFriendRequestAsync` to accept incoming friend requests:

!!! example
    ```csharp
    async void AcceptFriendRequest(int friendshipId)
    {
        try
        {
            var response = await GameFuseUser.CurrentUser.AcceptFriendRequestAsync(friendshipId);
            Debug.Log("Friend request accepted successfully!");
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Failed to accept friend request: {ex.Message}");
        }
    }
    ```

### Decline Friend Requests

Use `DeclineFriendRequestAsync` to decline incoming friend requests:

!!! example
    ```csharp
    async void DeclineFriendRequest(int friendshipId)
    {
        try
        {
            var response = await GameFuseUser.CurrentUser.DeclineFriendRequestAsync(friendshipId);
            Debug.Log("Friend request declined successfully!");
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Failed to decline friend request: {ex.Message}");
        }
    }
    ```

### Cancel Outgoing Friend Requests

Use `CancelFriendRequestAsync` to cancel friend requests you've sent:

!!! example
    ```csharp
    async void CancelFriendRequest(int friendshipId)
    {
        try
        {
            var response = await GameFuseUser.CurrentUser.CancelFriendRequestAsync(friendshipId);
            Debug.Log("Friend request cancelled successfully!");
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Failed to cancel friend request: {ex.Message}");
        }
    }
    ```

## Getting Friends and Friend Requests

### Get Friends List

Use `GetFriendsListAsync` to retrieve the current user's friends:

!!! example
    ```csharp
    async void LoadFriends()
    {
        try
        {
            var friends = await GameFuseUser.CurrentUser.GetFriendsListAsync();
            Debug.Log($"You have {friends.Count} friends");
            
            foreach (var friend in friends)
            {
                Debug.Log($"Friend: {friend.Username} (ID: {friend.Id})");
            }
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Failed to get friends list: {ex.Message}");
        }
    }
    ```

### Get Another User's Friends

Use `GetFriendsListForOtherUserAsync` to retrieve another user's friends list:

!!! example
    ```csharp
    async void LoadOtherUserFriends(int otherUserId)
    {
        try
        {
            var friends = await GameFuseUser.CurrentUser.GetFriendsListForOtherUserAsync(otherUserId);
            Debug.Log($"User {otherUserId} has {friends.Count} friends");
            
            foreach (var friend in friends)
            {
                Debug.Log($"Friend: {friend.Username} (ID: {friend.Id})");
            }
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Failed to get other user's friends: {ex.Message}");
        }
    }
    ```

### Get Incoming Friend Requests

Use `GetIncomingFriendRequestsAsync` to retrieve pending friend requests sent to you:

!!! example
    ```csharp
    async void LoadIncomingRequests()
    {
        try
        {
            var incomingRequests = await GameFuseUser.CurrentUser.GetIncomingFriendRequestsAsync();
            Debug.Log($"You have {incomingRequests.Count} incoming friend requests");
            
            foreach (var request in incomingRequests)
            {
                Debug.Log($"Request from: {request.Username} (Friendship ID: {request.FriendshipId})");
            }
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Failed to get incoming requests: {ex.Message}");
        }
    }
    ```

### Get Outgoing Friend Requests

Use `GetOutgoingFriendRequestsAsync` to retrieve friend requests you've sent:

!!! example
    ```csharp
    async void LoadOutgoingRequests()
    {
        try
        {
            var outgoingRequests = await GameFuseUser.CurrentUser.GetOutgoingFriendRequestsAsync();
            Debug.Log($"You have {outgoingRequests.Count} outgoing friend requests");
            
            foreach (var request in outgoingRequests)
            {
                Debug.Log($"Request to: {request.Username} (Friendship ID: {request.FriendshipId})");
            }
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Failed to get outgoing requests: {ex.Message}");
        }
    }
    ```

### Get All Friendship Data

Use `GetFriendshipDataAsync` to retrieve all friendship information at once (friends, incoming requests, outgoing requests):

!!! example
    ```csharp
    async void LoadAllFriendshipData()
    {
        try
        {
            var friendshipData = await GameFuseUser.CurrentUser.GetFriendshipDataAsync();
            
            Debug.Log($"Friends: {friendshipData.Friends.Count}");
            Debug.Log($"Incoming requests: {friendshipData.IncomingFriendRequests.Count}");
            Debug.Log($"Outgoing requests: {friendshipData.OutgoingFriendRequests.Count}");
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Failed to get friendship data: {ex.Message}");
        }
    }
    ```

## Removing Friends

Use `UnfriendPlayerAsync` to remove a friend from your friends list:

!!! example
    ```csharp
    async void RemoveFriend(int friendUserId)
    {
        try
        {
            var response = await GameFuseUser.CurrentUser.UnfriendPlayerAsync(friendUserId);
            Debug.Log("Friend removed successfully!");
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Failed to remove friend: {ex.Message}");
        }
    }
    ```

## Complete Example

Here's a comprehensive example showing how to implement a friendship system:

!!! example
    ```csharp
    using System.Collections.Generic;
    using UnityEngine;
    using UnityEngine.UI;
    using GameFuse;
    using GameFuse.Models.Shared;
    
    public class FriendshipManager : MonoBehaviour
    {
        [Header("UI References")]
        public InputField usernameInput;
        public Button sendRequestButton;
        public Transform friendsListParent;
        public Transform incomingRequestsParent;
        public Transform outgoingRequestsParent;
        public GameObject friendItemPrefab;
        public GameObject requestItemPrefab;
        
        void Start()
        {
            sendRequestButton.onClick.AddListener(OnSendRequestClicked);
            RefreshAllFriendshipData();
        }
        
        async void OnSendRequestClicked()
        {
            string username = usernameInput.text.Trim();
            if (string.IsNullOrEmpty(username))
            {
                Debug.LogWarning("Please enter a username");
                return;
            }
            
            await SendFriendRequest(username);
        }
        
        public async System.Threading.Tasks.Task SendFriendRequest(string username)
        {
            try
            {
                var response = await GameFuseUser.CurrentUser.SendFriendRequestAsync(username);
                Debug.Log($"Friend request sent to {username}");
                usernameInput.text = "";
                await RefreshAllFriendshipData();
            }
            catch (System.Exception ex)
            {
                Debug.LogError($"Error sending friend request: {ex.Message}");
            }
        }
        
        public async System.Threading.Tasks.Task RefreshAllFriendshipData()
        {
            try
            {
                var friendshipData = await GameFuseUser.CurrentUser.GetFriendshipDataAsync();
                
                UpdateFriendsList(friendshipData.Friends);
                UpdateIncomingRequests(friendshipData.IncomingFriendRequests);
                UpdateOutgoingRequests(friendshipData.OutgoingFriendRequests);
            }
            catch (System.Exception ex)
            {
                Debug.LogError($"Error refreshing friendship data: {ex.Message}");
            }
        }
        
        void UpdateFriendsList(IReadOnlyList<Friend> friends)
        {
            ClearParent(friendsListParent);
            
            foreach (var friend in friends)
            {
                var friendItem = Instantiate(friendItemPrefab, friendsListParent);
                var friendUI = friendItem.GetComponent<FriendItemUI>();
                if (friendUI != null)
                {
                    friendUI.Setup(friend, this);
                }
            }
        }
        
        void UpdateIncomingRequests(IReadOnlyList<FriendRequest> requests)
        {
            ClearParent(incomingRequestsParent);
            
            foreach (var request in requests)
            {
                var requestItem = Instantiate(requestItemPrefab, incomingRequestsParent);
                var requestUI = requestItem.GetComponent<IncomingRequestUI>();
                if (requestUI != null)
                {
                    requestUI.Setup(request, this);
                }
            }
        }
        
        void UpdateOutgoingRequests(IReadOnlyList<FriendRequest> requests)
        {
            ClearParent(outgoingRequestsParent);
            
            foreach (var request in requests)
            {
                var requestItem = Instantiate(requestItemPrefab, outgoingRequestsParent);
                var requestUI = requestItem.GetComponent<OutgoingRequestUI>();
                if (requestUI != null)
                {
                    requestUI.Setup(request, this);
                }
            }
        }
        
        void ClearParent(Transform parent)
        {
            foreach (Transform child in parent)
            {
                Destroy(child.gameObject);
            }
        }
        
        public async System.Threading.Tasks.Task AcceptFriendRequest(int friendshipId)
        {
            try
            {
                await GameFuseUser.CurrentUser.AcceptFriendRequestAsync(friendshipId);
                Debug.Log("Friend request accepted");
                await RefreshAllFriendshipData();
            }
            catch (System.Exception ex)
            {
                Debug.LogError($"Error accepting friend request: {ex.Message}");
            }
        }
        
        public async System.Threading.Tasks.Task DeclineFriendRequest(int friendshipId)
        {
            try
            {
                await GameFuseUser.CurrentUser.DeclineFriendRequestAsync(friendshipId);
                Debug.Log("Friend request declined");
                await RefreshAllFriendshipData();
            }
            catch (System.Exception ex)
            {
                Debug.LogError($"Error declining friend request: {ex.Message}");
            }
        }
        
        public async System.Threading.Tasks.Task CancelFriendRequest(int friendshipId)
        {
            try
            {
                await GameFuseUser.CurrentUser.CancelFriendRequestAsync(friendshipId);
                Debug.Log("Friend request cancelled");
                await RefreshAllFriendshipData();
            }
            catch (System.Exception ex)
            {
                Debug.LogError($"Error cancelling friend request: {ex.Message}");
            }
        }
        
        public async System.Threading.Tasks.Task RemoveFriend(int friendUserId)
        {
            try
            {
                await GameFuseUser.CurrentUser.UnfriendPlayerAsync(friendUserId);
                Debug.Log("Friend removed");
                await RefreshAllFriendshipData();
            }
            catch (System.Exception ex)
            {
                Debug.LogError($"Error removing friend: {ex.Message}");
            }
        }
    }
    ```

## Method Reference

### `SendFriendRequestAsync`
Sends a friend request to another player by username.

**Parameters:**
- `friendUsername` (string): Username of the player to send the request to
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<FriendshipResponse>` - Response containing friendship ID and message

### `AcceptFriendRequestAsync`
Accepts a pending friend request.

**Parameters:**
- `friendshipId` (int): ID of the friendship request to accept
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<FriendshipStatusResponse>` - Response confirming acceptance

### `DeclineFriendRequestAsync`
Declines a pending friend request.

**Parameters:**
- `friendshipId` (int): ID of the friendship request to decline
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<FriendshipStatusResponse>` - Response confirming decline

### `CancelFriendRequestAsync`
Cancels a friend request previously sent by this user.

**Parameters:**
- `friendshipId` (int): ID of the friendship request to cancel
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<FriendshipStatusResponse>` - Response confirming cancellation

### `GetFriendsListAsync`
Retrieves the current user's friends list and updates internal state.

**Parameters:**
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<IReadOnlyList<Friend>>` - Read-only list of friends

### `GetFriendsListForOtherUserAsync`
Retrieves another user's friends list.

**Parameters:**
- `otherUserId` (int): ID of the user whose friends to retrieve
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<IReadOnlyList<Friend>>` - Read-only list of the other user's friends

### `GetIncomingFriendRequestsAsync`
Retrieves incoming friend requests for the current user.

**Parameters:**
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<IReadOnlyList<FriendRequest>>` - Read-only list of incoming requests

### `GetOutgoingFriendRequestsAsync`
Retrieves outgoing friend requests for the current user.

**Parameters:**
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<IReadOnlyList<FriendRequest>>` - Read-only list of outgoing requests

### `GetFriendshipDataAsync`
Retrieves all friendship data (friends, incoming requests, outgoing requests) and updates internal state.

**Parameters:**
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<FriendshipDataResponse>` - Complete friendship data

### `UnfriendPlayerAsync`
Removes a friend from the current user's friends list.

**Parameters:**
- `friendUserId` (int): ID of the user to unfriend
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<FriendshipResponse>` - Response confirming unfriend action

## Error Handling

All friendship methods are async and may throw exceptions. Always wrap calls in try-catch blocks:

- **Network connectivity issues** - Connection problems
- **Authentication errors** - User not signed in
- **Invalid parameters** - User not found, invalid friendship ID
- **Permission errors** - Attempting actions not allowed for the current user
- **Server errors** - Temporary GameFuse service issues

## Function return values

### HTTP Status Codes

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK - Operation completed successfully |
| `400`            | Bad Request - Invalid parameters |
| `401`            | Unauthorized - User not authenticated |
| `403`            | Forbidden - Insufficient permissions |
| `404`            | Not Found - User or friendship not found |
| `409`            | Conflict - Friendship already exists or invalid state |
| `500`            | Internal Server Error - Server error |