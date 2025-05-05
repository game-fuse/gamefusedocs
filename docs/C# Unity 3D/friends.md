# Friendship System

GameFuse provides a comprehensive friendship system that allows players to connect with each other. The `GameFuseUser.Friends` partial class contains methods to manage friend relationships within your game.

## Friend System Methods

All friendship methods are accessible through the `GameFuseUser` instance after a successful login.

### SendFriendRequestAsync

Sends a friend request to another player by username.

**Parameters:**
- `otherUserName` (string): Username of the player to send the friend request to

**Returns:**
- `Task<FriendRequestResponse>`: Response containing:
  - `friendship_id`: Unique ID for the friendship request
  - Confirmation message

**Example:**
```csharp
try
{
    // Get the current authenticated user
    GameFuseUser currentUser = GameFuseUser.CurrentUser;
    
    // Send a friend request to another player
    FriendRequestResponse response = await currentUser.SendFriendRequestAsync("playerUsername");
    
    // Access the friendship ID
    int friendshipId = response.response.FriendshipId;
    Debug.Log($"Friend request sent successfully! Friendship ID: {friendshipId}");
}
catch (ApiException ex)
{
    Debug.LogError($"Failed to send friend request: {ex.Message}");
}
```

### CancelFriendRequestAsync

Cancels a pending friend request that was previously sent. Only the user who sent the request can cancel it.

**Parameters:**
- `friendshipId` (int): ID of the friendship request to cancel

**Returns:**
- `Task<FriendshipStatusResponse>`: Response confirming the cancellation

**Example:**
```csharp
try
{
    // Get the current authenticated user
    GameFuseUser currentUser = GameFuseUser.CurrentUser;
    
    // Cancel a pending outgoing friend request
    FriendshipStatusResponse response = await currentUser.CancelFriendRequestAsync(12345);
    
    Debug.Log("Friend request cancelled successfully!");
}
catch (ApiException ex)
{
    Debug.LogError($"Failed to cancel friend request: {ex.Message}");
}
```

### AcceptFriendRequestAsync

Accepts a pending friend request. Only the request recipient can accept it.

**Parameters:**
- `friendshipId` (int): ID of the friendship request to accept

**Returns:**
- `Task<FriendshipStatusResponse>`: Response confirming the acceptance

**Example:**
```csharp
try
{
    // Get the current authenticated user
    GameFuseUser currentUser = GameFuseUser.CurrentUser;
    
    // Accept an incoming friend request
    FriendshipStatusResponse response = await currentUser.AcceptFriendRequestAsync(12345);
    
    Debug.Log("Friend request accepted successfully!");
}
catch (ApiException ex)
{
    Debug.LogError($"Failed to accept friend request: {ex.Message}");
}
```

### DeclineFriendRequestAsync

Declines a pending friend request. Only the request recipient can decline it.

**Parameters:**
- `friendshipId` (int): ID of the friendship request to decline

**Returns:**
- `Task<FriendshipStatusResponse>`: Response confirming the decline

**Example:**
```csharp
try
{
    // Get the current authenticated user
    GameFuseUser currentUser = GameFuseUser.CurrentUser;
    
    // Decline an incoming friend request
    FriendshipStatusResponse response = await currentUser.DeclineFriendRequestAsync(12345);
    
    Debug.Log("Friend request declined successfully!");
}
catch (ApiException ex)
{
    Debug.LogError($"Failed to decline friend request: {ex.Message}");
}
```

### GetFriendsAsync

Gets the list of current friends for this user.

**Parameters:**
- None

**Returns:**
- `Task<UserInfo[]>`: Array of UserInfo objects containing friend details (username, ID, etc.)

**Example:**
```csharp
try
{
    // Get the current authenticated user
    GameFuseUser currentUser = GameFuseUser.CurrentUser;
    
    // Get the list of friends
    UserInfo[] friends = await currentUser.GetFriendsAsync();
    
    // Display the friends
    foreach (UserInfo friend in friends)
    {
        Debug.Log($"Friend: {friend.Username} (ID: {friend.Id})");
    }
}
catch (ApiException ex)
{
    Debug.LogError($"Failed to get friends list: {ex.Message}");
}
```

### GetIncomingFriendRequestsAsync

Gets the list of pending friend requests sent to this user.

**Parameters:**
- None

**Returns:**
- `Task<FriendRequest[]>`: Array of FriendRequest objects containing incoming request details

**Example:**
```csharp
try
{
    // Get the current authenticated user
    GameFuseUser currentUser = GameFuseUser.CurrentUser;
    
    // Get incoming friend requests
    FriendRequest[] incomingRequests = await currentUser.GetIncomingFriendRequestsAsync();
    
    // Display the incoming requests
    foreach (FriendRequest request in incomingRequests)
    {
        Debug.Log($"Incoming request from {request.Username} (ID: {request.Id}, Friendship ID: {request.FriendshipId})");
    }
}
catch (ApiException ex)
{
    Debug.LogError($"Failed to get incoming friend requests: {ex.Message}");
}
```

### GetOutgoingFriendRequestsAsync

Gets the list of pending friend requests sent by this user.

**Parameters:**
- None

**Returns:**
- `Task<FriendRequest[]>`: Array of FriendRequest objects containing outgoing request details

**Example:**
```csharp
try
{
    // Get the current authenticated user
    GameFuseUser currentUser = GameFuseUser.CurrentUser;
    
    // Get outgoing friend requests
    FriendRequest[] outgoingRequests = await currentUser.GetOutgoingFriendRequestsAsync();
    
    // Display the outgoing requests
    foreach (FriendRequest request in outgoingRequests)
    {
         Debug.Log($"Outgoing request to {request.Username} (ID: {request.Id}, Friendship ID: {request.FriendshipId})");
    }
}
catch (ApiException ex)
{
    Debug.LogError($"Failed to get outgoing friend requests: {ex.Message}");
}
```

### UnFriendAsync

Removes a friend from the user's friend list.

**Parameters:**
- `userId` (int): ID of the user to unfriend

**Returns:**
- `Task<FriendshipStatusResponse>`: Response confirming the unfriend action

**Example:**
```csharp
try
{
    // Get the current authenticated user
    GameFuseUser currentUser = GameFuseUser.CurrentUser;
    
    // Remove a friend
    FriendshipStatusResponse response = await currentUser.UnFriendAsync(12345);
    
    Debug.Log("Friend removed successfully!");
}
catch (ApiException ex)
{
    Debug.LogError($"Failed to unfriend player: {ex.Message}");
}
```

## Common Usage Pattern

Here's an example of how to implement a basic friend system in your game:

```csharp
using UnityEngine;
using GameFuseCSharp;

public class FriendManager : MonoBehaviour
{
    // UI elements for friend list
    [SerializeField] private Transform friendListContainer;
    [SerializeField] private GameObject friendItemPrefab;
    
    // Load friends when the panel is opened
    public async void LoadFriends()
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
            foreach (Transform child in friendListContainer)
            {
                Destroy(child.gameObject);
            }
            
            // Get all friends
            UserInfo[] friends = await currentUser.GetFriendsAsync();
            
            // Create UI items for each friend
            foreach (UserInfo friend in friends)
            {
                GameObject friendItem = Instantiate(friendItemPrefab, friendListContainer);
                FriendUI friendUI = friendItem.GetComponent<FriendUI>();
                friendUI.SetUserInfo(friend);
            }
        }
        catch (ApiException ex)
        {
            Debug.LogError($"Error loading friends: {ex.Message}");
        }
    }
    
    // Send a friend request
    public async void SendFriendRequest(string username)
    {
        try
        {
            GameFuseUser currentUser = GameFuseUser.CurrentUser;
            if (currentUser == null)
            {
                Debug.LogError("User not logged in");
                return;
            }
            
            FriendRequestResponse response = await currentUser.SendFriendRequestAsync(username);
            Debug.Log($"Friend request sent to {username}");
        }
        catch (ApiException ex)
        {
            Debug.LogError($"Error sending friend request: {ex.Message}");
        }
    }
    
    // Remove a friend
    public async void RemoveFriend(int friendId, string friendName)
    {
        try
        {
            GameFuseUser currentUser = GameFuseUser.CurrentUser;
            if (currentUser == null)
            {
                Debug.LogError("User not logged in");
                return;
            }
            
            await currentUser.UnFriendAsync(friendId);
            Debug.Log($"Removed {friendName} from friends");
            
            // Refresh the friend list
            LoadFriends();
        }
        catch (ApiException ex)
        {
            Debug.LogError($"Error removing friend: {ex.Message}");
        }
    }

    // Handle incoming friend requests
    public async void LoadIncomingFriendRequests(Transform incomingRequestsContainer, GameObject requestItemPrefab)
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
            foreach (Transform child in incomingRequestsContainer)
            {
                Destroy(child.gameObject);
            }

            // Get incoming friend requests
            FriendRequest[] incomingRequests = await currentUser.GetIncomingFriendRequestsAsync();

            // Create UI items for each request
            foreach (FriendRequest request in incomingRequests)
            {
                GameObject requestItem = Instantiate(requestItemPrefab, incomingRequestsContainer);
                IncomingFriendRequestUI requestUI = requestItem.GetComponent<IncomingFriendRequestUI>();
                if (requestUI != null)
                {
                    requestUI.SetFriendRequest(request);
                }
            }
        }
        catch (ApiException ex)
        {
            Debug.LogError($"Error loading incoming friend requests: {ex.Message}");
        }
    }

    // Accept a friend request
    public async void AcceptFriendRequest(int friendshipId)
    {
        try
        {
            GameFuseUser currentUser = GameFuseUser.CurrentUser;
            if (currentUser == null)
            {
                Debug.LogError("User not logged in");
                return;
            }

            FriendshipStatusResponse response = await currentUser.AcceptFriendRequestAsync(friendshipId);
            Debug.Log("Friend request accepted successfully!");
            
            // Refresh the friend list
            LoadFriends();
        }
        catch (ApiException ex)
        {
            Debug.LogError($"Error accepting friend request: {ex.Message}");
        }
    }

    // Decline a friend request
    public async void DeclineFriendRequest(int friendshipId)
    {
        try
        {
            GameFuseUser currentUser = GameFuseUser.CurrentUser;
            if (currentUser == null)
            {
                Debug.LogError("User not logged in");
                return;
            }

            FriendshipStatusResponse response = await currentUser.DeclineFriendRequestAsync(friendshipId);
            Debug.Log("Friend request declined successfully!");
        }
        catch (ApiException ex)
        {
            Debug.LogError($"Error declining friend request: {ex.Message}");
        }
    }
}
```

## Error Handling

All friendship methods throw an `ApiException` when a request fails. Always wrap your API calls in try-catch blocks to handle errors gracefully.

Common errors include:
- User not found (when sending a friend request)
- Insufficient permissions (trying to accept a request you didn't receive)
- Network connectivity issues