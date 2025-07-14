# Managing Friends

GameFuse provides methods to manage friends, including retrieving the list of friends, incoming friend requests, and outgoing friend requests. This documentation covers how to use these methods.

!!! note
    This feature is not supported in the js client library yet.
	

## Table of Contents
- [Retrieving Friends](#retrieving-friends)
- [Retrieving Incoming Friend Requests](#retrieving-incoming-friend-requests)
- [Retrieving Outgoing Friend Requests](#retrieving-outgoing-friend-requests)
- [Sending Friend Requests](#sending-friend-requests)
- [Handling Friend Requests](#handling-friend-requests)
  - [Accepting Friend Requests](#accepting-friend-requests)
  - [Declining Friend Requests](#declining-friend-requests)
  - [Cancelling Friend Requests](#cancelling-friend-requests)
- [Removing Friends](#removing-friends)
- [Example Usage](#example-usage)

---
## Retrieving Friends

To retrieve the list of friends for the current user:

```jsx
// Retrieve the list of friends
const friends = GameFuseUser.CurrentUser.getFriends();
console.log("Friends: ", friends);
```

## Retrieving Incoming Friend Requests

To retrieve the list of incoming friend requests for the current user:

```jsx
// Retrieve the list of incoming friend requests
const incomingFriendRequests = GameFuseUser.CurrentUser.getIncomingFriendRequests();
console.log("Incoming Friend Requests: ", incomingFriendRequests);
```

## Retrieving Outgoing Friend Requests

To retrieve the list of outgoing friend requests for the current user:

```jsx
// Retrieve the list of outgoing friend requests
const outgoingFriendRequests = GameFuseUser.CurrentUser.getOutgoingFriendRequests();
console.log("Outgoing Friend Requests: ", outgoingFriendRequests);
```

## Sending Friend Requests

### Sending Friend Requests by User Name

To send a friend request by user name:

```jsx
// Send a friend request by username
async function sendFriendRequestByUsername(username) {
    try {
        await GameFuseFriendRequest.send(username);
        console.log("Friend request sent successfully");
    } catch (error) {
        console.error("Failed to send friend request:", error);
    }
}

// Usage example
sendFriendRequestByUsername("otherUsername");
```

### Sending Friend Requests by User Object

To send a friend request using a GameFuseUser object:

```jsx
// Send a friend request using a GameFuseUser object
async function sendFriendRequestByUser(user) {
    try {
        await user.sendFriendRequest();
        console.log(`Friend request sent to ${user.getUsername()}`);
    } catch (error) {
        console.error("Failed to send friend request:", error);
    }
}

// Assume 'otherUser' is a GameFuseUser object representing another user
const otherUser = new GameFuseUser(/* user details */);
sendFriendRequestByUser(otherUser);
```
## Handling Friend Requests

You can also handle friend requests by accepting declining or cancelling them.

### Accepting Friend Requests
Here is an example of how to accept an incoming friend request:

```jsx
// Accept an incoming friend request
async function acceptFriendRequest(friendRequest) {
    try {
        await friendRequest.accept();
        const senderUsername = friendRequest.getOtherUser().getUsername();
        console.log(`Friend request from ${senderUsername} accepted`);
    } catch (error) {
        console.error("Failed to accept friend request:", error);
    }
}

// Usage example
// Accept all incoming friend requests
async function acceptAllFriendRequests() {
    const incomingRequests = GameFuseUser.CurrentUser.getIncomingFriendRequests();
    if (incomingRequests.length > 0) {
        for (const request of incomingRequests) {
            await acceptFriendRequest(request);
        }
    } else {
        console.log("No incoming friend requests");
    }
}

acceptAllFriendRequests();
```

### Declining Friend Requests

Similarly, you can decline a friend request:

```jsx
// Decline an incoming friend request
async function declineFriendRequest(friendRequest) {
    try {
        await friendRequest.decline();
        const senderUsername = friendRequest.getOtherUser().getUsername();
        console.log(`Friend request from ${senderUsername} declined`);
    } catch (error) {
        console.error("Failed to decline friend request:", error);
    }
}

// Usage example
async function handleIncomingRequests() {
    const incomingRequests = GameFuseUser.CurrentUser.getIncomingFriendRequests();
    if (incomingRequests.length > 0) {
        await declineFriendRequest(incomingRequests[0]);
    } else {
        console.log("No incoming friend requests");
    }
}

handleIncomingRequests();
```

### Cancelling Friend Requests

Finally, you can cancel an outgoing friend request:

```jsx
// Cancel an outgoing friend request
async function cancelFriendRequest(friendRequest) {
    try {
        await friendRequest.cancel();
        const receiverUsername = friendRequest.getOtherUser().getUsername();
        console.log(`Friend request to ${receiverUsername} canceled`);
    } catch (error) {
        console.error("Failed to cancel friend request:", error);
    }
}

// Usage example
async function handleOutgoingRequests() {
    const outgoingRequests = GameFuseUser.CurrentUser.getOutgoingFriendRequests();
    if (outgoingRequests.length > 0) {
        await cancelFriendRequest(outgoingRequests[0]);
    } else {
        console.log("No outgoing friend requests to cancel");
    }
}

handleOutgoingRequests();
``` 
## Removing Friends

To remove a user from your friends list:

```jsx
async function unfriendUser(user) {
    try {
        await user.unfriend();
        console.log(`User ${user.getUsername()} unfriended successfully`);
    } catch (error) {
        console.error(`Failed to unfriend user ${user.getUsername()}:`, error);
    }
}

// Example usage
async function exampleUnfriendUsage() {
    // Retrieve the list of friends
    const friends = GameFuseUser.CurrentUser.getFriends();
    
    if (friends.length > 0) {
        // Unfriend the first user in the friends list
        const friendToUnfriend = friends[0];
        await unfriendUser(friendToUnfriend);
    } else {
        console.log("No friends available to unfriend");
    }
}

// Call the example usage function
exampleUnfriendUsage();
```

## Example Usage

Here is an example of how to use the above methods to manage friends:

```jsx
// Example usage of the above methods
async function manageFriends() {
    try {
        // Send a friend request
        await GameFuseFriendRequest.send("myNewFriend");
        console.log("Friend request sent successfully");

        // Monitor the status of outgoing friend requests
        let outgoingRequests = GameFuseUser.CurrentUser.getOutgoingFriendRequests();
        while (outgoingRequests.length > 0) {
            console.log("Friend request is still pending...");
            // Wait for 1 second before checking again
            await new Promise(resolve => setTimeout(resolve, 1000));
            outgoingRequests = GameFuseUser.CurrentUser.getOutgoingFriendRequests();
        }

        // Retrieve the updated friends list after the request is accepted
        const friends = GameFuseUser.CurrentUser.getFriends();
        console.log("Friends:", friends);

        // Send a message to the new friend
        const friend = friends.find(user => user.getUsername() === "myNewFriend");
        if (friend) {
            await friend.sendMessage("Hello, how are you?");
            console.log("Message sent to friend");
        }

        // Remove the friend
        await friend.unfriend();
        console.log("Friend removed successfully");
    } catch (error) {
        console.error("An error occurred:", error);
    }
}

// Call the manageFriends function
manageFriends();
```


This documentation provides an overview of how to manage friends and handle friend requests using the GameFuse SDK.