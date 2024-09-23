# Managing Friends

GameFuse provides methods to manage friends, including retrieving the list of friends, incoming friend requests, and outgoing friend requests. This documentation covers how to use these methods.

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

## Example Usage

Here is an example of how to use these methods in a typical scenario:

```jsx
// Example usage of friend management methods
function manageFriends() {
    // Retrieve and log the list of friends
    const friends = GameFuseUser.CurrentUser.getFriends();
    console.log("Friends: ", friends);

    // Retrieve and log the list of incoming friend requests
    const incomingFriendRequests = GameFuseUser.CurrentUser.getIncomingFriendRequests();
    console.log("Incoming Friend Requests: ", incomingFriendRequests);

    // Retrieve and log the list of outgoing friend requests
    const outgoingFriendRequests = GameFuseUser.CurrentUser.getOutgoingFriendRequests();
    console.log("Outgoing Friend Requests: ", outgoingFriendRequests);
}

// Call the function to manage friends
manageFriends();
```

This example demonstrates how to retrieve and log the lists of friends, incoming friend requests, and outgoing friend requests for the current user.

## Handling Friend Requests

You can also handle friend requests by accepting or declining them. Here is an example of how to accept an incoming friend request:

```jsx
// Accept an incoming friend request
function acceptFriendRequest(friendRequest) {
    friendRequest.accept((response, success) => {
        if (success) {
            console.log("Friend request accepted");
        } else {
            console.error("Failed to accept friend request:", response);
        }
    });
}

// Example usage
const incomingRequests = GameFuseUser.CurrentUser.getIncomingFriendRequests();
if (incomingRequests.length > 0) {
    acceptFriendRequest(incomingRequests[0]);
}
```

Similarly, you can decline a friend request:

```jsx
// Decline an incoming friend request
function declineFriendRequest(friendRequest) {
    friendRequest.decline((response, success) => {
        if (success) {
            console.log("Friend request declined");
        } else {
            console.error("Failed to decline friend request:", response);
        }
    });
}

// Example usage
const incomingRequests = GameFuseUser.CurrentUser.getIncomingFriendRequests();
if (incomingRequests.length > 0) {
    declineFriendRequest(incomingRequests[0]);
}
```

This documentation provides an overview of how to manage friends and handle friend requests using the GameFuse SDK.