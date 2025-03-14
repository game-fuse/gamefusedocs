# Friends System

!!! note
    These Docs are WIP and will be updated with Visual Blueprint Examples. Please see [Class Methods](class%20methods.md) for visual examples.

The GameFuse Friends System allows you to implement social features in your game, such as sending friend requests, accepting or declining requests, and managing a friends list.

## Getting Started with Friends

To use the GameFuse Friends system in Blueprints, you'll need to access the GameFuse Friends subsystem:


<!-- 
<!-- <iframe src="https://blueprintue.com/render/your-blueprint-id/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

[Copy Code](https://blueprintue.com/blueprint/your-blueprint-id/) -->

## Sending Friend Requests

To send a friend request:

1. Get the GameFuse Friends subsystem
2. Call SendFriendRequest with the username
3. Create a callback function to handle the response
4. Process the response data

## Accepting Friend Requests

To accept a friend request:

1. Get the GameFuse Friends subsystem
2. Call AcceptFriendRequest with the friendship ID
3. Create a callback function to handle the response
4. Process the success or failure

## Declining Friend Requests

To decline a friend request:

1. Get the GameFuse Friends subsystem
2. Call DeclineFriendRequest with the friendship ID
3. Create a callback function to handle the response
4. Process the success or failure

## Canceling Friend Requests

To cancel a sent friend request:

1. Get the GameFuse Friends subsystem
2. Call CancelFriendRequest with the friendship ID
3. Create a callback function to handle the response
4. Process the success or failure

## Unfriending Players

To remove a user from your friends list:

1. Get the GameFuse Friends subsystem
2. Call UnfriendPlayer with the user ID
3. Create a callback function to handle the response
4. Process the success or failure

## Fetching Friendship Data

To fetch all friendship data:

1. Get the GameFuse Friends subsystem
2. Call FetchFriendshipData
3. Create a callback function to handle the response
4. Process the friends list, outgoing requests, and incoming requests

## Fetching Friends List

To fetch your friends list:

1. Get the GameFuse Friends subsystem
2. Call FetchFriendsList
3. Create a callback function to handle the response
4. Process the array of friends

## Fetching Friend Requests

To fetch outgoing friend requests:

1. Get the GameFuse Friends subsystem
2. Call FetchOutgoingFriendRequests
3. Create a callback function to handle the response
4. Process the array of outgoing requests

To fetch incoming friend requests:

1. Get the GameFuse Friends subsystem
2. Call FetchIncomingFriendRequests
3. Create a callback function to handle the response
4. Process the array of incoming requests 