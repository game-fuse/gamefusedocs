# Groups System

!!! note
    These Docs are WIP and will be updated with Visual Blueprint Examples. Please see [Class Methods](class%20methods.md) for visual examples.

The GameFuse Groups System allows you to create and manage groups of users in your game. This includes creating groups, joining groups, managing group membership, and handling group attributes.

## Getting Started with Groups

To use the GameFuse Groups system in Blueprints, you'll need to access the GameFuse Groups subsystem:


<!-- 
<!-- <iframe src="https://blueprintue.com/render/your-blueprint-id/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

[Copy Code](https://blueprintue.com/blueprint/your-blueprint-id/) -->

## Creating a Group

To create a new group:

1. Get the GameFuse Groups subsystem
2. Create a new Group structure
3. Set properties like Name, Group Type, Max Group Size, etc.
4. Call CreateGroup with the group data
5. Create a callback function to handle the response
6. Process the created group data

## Fetching a Group

To fetch a specific group:

1. Get the GameFuse Groups subsystem
2. Call FetchGroup with the group ID
3. Create a callback function to handle the response
4. Process the group data for display

## Fetching All Groups

To fetch all groups:

1. Get the GameFuse Groups subsystem
2. Call FetchAllGroups
3. Create a callback function to handle the response
4. Process the array of groups for display

## Joining a Group

To join a group:

1. Get the GameFuse Groups subsystem
2. Call JoinGroup with the group ID
3. Create a callback function to handle the response
4. Process the success or failure of joining the group

## Requesting to Join a Group

To request to join an invite-only group:

1. Get the GameFuse Groups subsystem
2. Call RequestToJoinGroup with the group ID
3. Create a callback function to handle the response
4. Process the success or failure of the join request

## Accepting Join Requests

To accept a join request:

1. Get the GameFuse Groups subsystem
2. Call AcceptJoinRequest with the group ID and user ID
3. Create a callback function to handle the response
4. Process the success or failure of accepting the join request

## Declining Join Requests

To decline a join request:

1. Get the GameFuse Groups subsystem
2. Call DeclineJoinRequest with the group ID and user ID
3. Create a callback function to handle the response
4. Process the success or failure of declining the join request

## Leaving a Group

To leave a group:

1. Get the GameFuse Groups subsystem
2. Call LeaveGroup with the group ID
3. Create a callback function to handle the response
4. Process the success or failure of leaving the group

## Deleting a Group

To delete a group:

1. Get the GameFuse Groups subsystem
2. Call DeleteGroup with the group ID
3. Create a callback function to handle the response
4. Process the success or failure of deleting the group

## Fetching User Groups

To fetch user's groups:

1. Get the GameFuse Groups subsystem
2. Call FetchUserGroups
3. Create a callback function to handle the response
4. Process the user's groups for display

## Searching for Groups

To search for groups:

1. Get the GameFuse Groups subsystem
2. Call SearchGroups with the search query
3. Create a callback function to handle the response
4. Process the search results for display

## Managing Group Admins

To add an admin to a group:

1. Get the GameFuse Groups subsystem
2. Call AddAdmin with the group ID and user ID
3. Create a callback function to handle the response
4. Process the success or failure of adding the admin

To remove an admin from a group:

1. Get the GameFuse Groups subsystem
2. Call RemoveAdmin with the group ID and user ID
3. Create a callback function to handle the response
4. Process the success or failure of removing the admin

## Managing Group Attributes

To add an attribute to a group:

1. Get the GameFuse Groups subsystem
2. Call AddAttribute with the group ID, key, and value
3. Create a callback function to handle the response
4. Process the success or failure of adding the attribute

To update an attribute:

1. Get the GameFuse Groups subsystem
2. Call UpdateAttribute with the group ID, key, and new value
3. Create a callback function to handle the response
4. Process the success or failure of updating the attribute

To remove an attribute:

1. Get the GameFuse Groups subsystem
2. Call RemoveAttribute with the group ID and key
3. Create a callback function to handle the response
4. Process the success or failure of removing the attribute 