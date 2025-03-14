# Friends System

The GameFuse Friends System allows you to implement social features in your game, such as sending friend requests, accepting or declining requests, and managing a friends list.

## Getting Started with Friends

To use the GameFuse Friends system in Blueprints, you'll need to access the GameFuse Friends subsystem:

<iframe src="https://blueprintue.com/render/your-blueprint-id/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

[Copy Code](https://blueprintue.com/blueprint/your-blueprint-id/)

## Sending Friend Requests

You can send a friend request to another user by their username:

<iframe src="https://blueprintue.com/render/your-blueprint-id/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

[Copy Code](https://blueprintue.com/blueprint/your-blueprint-id/)

The Blueprint should:

1. Get the GameFuse Friends subsystem
2. Call SendFriendRequest with the username
3. Create a callback function to handle the response
4. Process the response data

## Accepting Friend Requests

When someone sends you a friend request, you can accept it:

<iframe src="https://blueprintue.com/render/your-blueprint-id/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

[Copy Code](https://blueprintue.com/blueprint/your-blueprint-id/)

The Blueprint should:

1. Get the GameFuse Friends subsystem
2. Call AcceptFriendRequest with the friendship ID
3. Create a callback function to handle the response
4. Process the success or failure

## Declining Friend Requests

You can also decline a friend request:

<iframe src="https://blueprintue.com/render/your-blueprint-id/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

[Copy Code](https://blueprintue.com/blueprint/your-blueprint-id/)

The Blueprint should:

1. Get the GameFuse Friends subsystem
2. Call DeclineFriendRequest with the friendship ID
3. Create a callback function to handle the response
4. Process the success or failure

## Canceling Friend Requests

If you've sent a friend request and want to cancel it:

<iframe src="https://blueprintue.com/render/your-blueprint-id/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

[Copy Code](https://blueprintue.com/blueprint/your-blueprint-id/)

The Blueprint should:

1. Get the GameFuse Friends subsystem
2. Call CancelFriendRequest with the friendship ID
3. Create a callback function to handle the response
4. Process the success or failure

## Unfriending Players

You can remove a user from your friends list:

<iframe src="https://blueprintue.com/render/your-blueprint-id/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

[Copy Code](https://blueprintue.com/blueprint/your-blueprint-id/)

The Blueprint should:

1. Get the GameFuse Friends subsystem
2. Call UnfriendPlayer with the user ID
3. Create a callback function to handle the response
4. Process the success or failure

## Fetching Friendship Data

You can fetch all friendship data for the current user:

<iframe src="https://blueprintue.com/render/your-blueprint-id/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

[Copy Code](https://blueprintue.com/blueprint/your-blueprint-id/)

The Blueprint should:

1. Get the GameFuse Friends subsystem
2. Call FetchFriendshipData
3. Create a callback function to handle the response
4. Process the friends list, outgoing requests, and incoming requests

## Fetching Friends List

You can fetch just the friends list:

<iframe src="https://blueprintue.com/render/your-blueprint-id/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

[Copy Code](https://blueprintue.com/blueprint/your-blueprint-id/)

The Blueprint should:

1. Get the GameFuse Friends subsystem
2. Call FetchFriendsList
3. Create a callback function to handle the response
4. Process the array of friends

## Fetching Friend Requests

You can fetch outgoing friend requests:

<iframe src="https://blueprintue.com/render/your-blueprint-id/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

[Copy Code](https://blueprintue.com/blueprint/your-blueprint-id/)

The Blueprint should:

1. Get the GameFuse Friends subsystem
2. Call FetchOutgoingFriendRequests
3. Create a callback function to handle the response
4. Process the array of outgoing requests

And incoming friend requests:

<iframe src="https://blueprintue.com/render/your-blueprint-id/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

[Copy Code](https://blueprintue.com/blueprint/your-blueprint-id/)

The Blueprint should:

1. Get the GameFuse Friends subsystem
2. Call FetchIncomingFriendRequests
3. Create a callback function to handle the response
4. Process the array of incoming requests 