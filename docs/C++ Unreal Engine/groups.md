# Groups System

The GameFuse Groups System allows you to create and manage groups of users in your game. This includes creating groups, joining groups, managing group membership, and handling group attributes.

## Getting Started with Groups

To use the GameFuse Groups system, you'll need to access the `UGameFuseGroups` subsystem:

```cpp
UGameFuseGroups* GroupsSystem = GetGameInstance()->GetSubsystem<UGameFuseGroups>();
```

## Creating a Group

You can create a new group with specific settings:

```cpp
void UMyGameMode::CreateGroup()
{
    UGameFuseGroups* GroupsSystem = GetGameInstance()->GetSubsystem<UGameFuseGroups>();
    
    // Create a new group structure
    FGFGroup GroupData;
    GroupData.Name = "My Awesome Group";
    GroupData.GroupType = "Clan";
    GroupData.MaxGroupSize = 10;
    GroupData.bCanAutoJoin = false;
    GroupData.bIsInviteOnly = true;
    GroupData.bSearchable = true;
    GroupData.bAdminsOnlyCanCreateAttributes = true;
    
    // Create typed callback
    FGFGroupCallback OnCreateGroup;
    OnCreateGroup.BindLambda([this](const FGFGroup& CreatedGroup) {
        // Use CreatedGroup data (ID, name, settings, etc.)
    });
    
    // Create the group
    GroupsSystem->CreateGroup(GroupData, OnCreateGroup);
}
```

## Fetching a Group

You can fetch a specific group by its ID:

```cpp
void UMyGameMode::FetchGroup(int32 GroupId)
{
    UGameFuseGroups* GroupsSystem = GetGameInstance()->GetSubsystem<UGameFuseGroups>();
    
    // Create typed callback
    FGFGroupCallback OnFetchGroup;
    OnFetchGroup.BindLambda([this](const FGFGroup& Group) {
        // Use Group data to display group details
    });
    
    // Fetch the group
    GroupsSystem->FetchGroup(GroupId, OnFetchGroup);
}
```

## Fetching All Groups

You can fetch all groups in the game:

```cpp
void UMyGameMode::FetchAllGroups()
{
    UGameFuseGroups* GroupsSystem = GetGameInstance()->GetSubsystem<UGameFuseGroups>();
    
    // Create typed callback
    FGFGroupsCallback OnFetchAllGroups;
    OnFetchAllGroups.BindLambda([this](const TArray<FGFGroup>& Groups) {
        // Process array of groups for display
    });
    
    // Fetch all groups
    GroupsSystem->FetchAllGroups(OnFetchAllGroups);
}
```

## Joining a Group

You can join a group:

```cpp
void UMyGameMode::JoinGroup(int32 GroupId)
{
    UGameFuseGroups* GroupsSystem = GetGameInstance()->GetSubsystem<UGameFuseGroups>();
    
    // Create typed callback
    FGFGroupActionCallback OnJoinGroup;
    OnJoinGroup.BindLambda([this, GroupId](bool bSuccess) {
        // Handle success or failure of joining group
    });
    
    // Join the group
    GroupsSystem->JoinGroup(GroupId, OnJoinGroup);
}
```

## Requesting to Join a Group

For invite-only groups, you can request to join:

```cpp
void UMyGameMode::RequestToJoinGroup(int32 GroupId)
{
    UGameFuseGroups* GroupsSystem = GetGameInstance()->GetSubsystem<UGameFuseGroups>();
    
    // Create typed callback
    FGFGroupActionCallback OnRequestToJoinGroup;
    OnRequestToJoinGroup.BindLambda([this, GroupId](bool bSuccess) {
        // Handle success or failure of join request
    });
    
    // Request to join the group
    GroupsSystem->RequestToJoinGroup(GroupId, OnRequestToJoinGroup);
}
```

## Accepting Join Requests

As a group admin, you can accept join requests:

```cpp
void UMyGameMode::AcceptJoinRequest(int32 GroupId, int32 UserId)
{
    UGameFuseGroups* GroupsSystem = GetGameInstance()->GetSubsystem<UGameFuseGroups>();
    
    // Create typed callback
    FGFGroupActionCallback OnAcceptJoinRequest;
    OnAcceptJoinRequest.BindLambda([this, GroupId, UserId](bool bSuccess) {
        // Handle success or failure of accepting join request
    });
    
    // Accept the join request
    GroupsSystem->AcceptJoinRequest(GroupId, UserId, OnAcceptJoinRequest);
}
```

## Declining Join Requests

As a group admin, you can decline join requests:

```cpp
void UMyGameMode::DeclineJoinRequest(int32 GroupId, int32 UserId)
{
    UGameFuseGroups* GroupsSystem = GetGameInstance()->GetSubsystem<UGameFuseGroups>();
    
    // Create typed callback
    FGFGroupActionCallback OnDeclineJoinRequest;
    OnDeclineJoinRequest.BindLambda([this, UserId](bool bSuccess) {
        // Handle success or failure of declining join request
    });
    
    // Decline the join request
    GroupsSystem->DeclineJoinRequest(GroupId, UserId, OnDeclineJoinRequest);
}
```

## Leaving a Group

You can leave a group you've joined:

```cpp
void UMyGameMode::LeaveGroup(int32 GroupId)
{
    UGameFuseGroups* GroupsSystem = GetGameInstance()->GetSubsystem<UGameFuseGroups>();
    
    // Create typed callback
    FGFGroupActionCallback OnLeaveGroup;
    OnLeaveGroup.BindLambda([this, GroupId](bool bSuccess) {
        // Handle success or failure of leaving group
    });
    
    // Leave the group
    GroupsSystem->LeaveGroup(GroupId, OnLeaveGroup);
}
```

## Deleting a Group

If you're the creator or an admin, you can delete a group:

```cpp
void UMyGameMode::DeleteGroup(int32 GroupId)
{
    UGameFuseGroups* GroupsSystem = GetGameInstance()->GetSubsystem<UGameFuseGroups>();
    
    // Create typed callback
    FGFGroupActionCallback OnDeleteGroup;
    OnDeleteGroup.BindLambda([this, GroupId](bool bSuccess) {
        // Handle success or failure of deleting group
    });
    
    // Delete the group
    GroupsSystem->DeleteGroup(GroupId, OnDeleteGroup);
}
```

## Fetching User Groups

You can fetch all groups that the current user is a member of:

```cpp
void UMyGameMode::FetchUserGroups()
{
    UGameFuseGroups* GroupsSystem = GetGameInstance()->GetSubsystem<UGameFuseGroups>();
    
    // Create typed callback
    FGFGroupsCallback OnFetchUserGroups;
    OnFetchUserGroups.BindLambda([this](const TArray<FGFGroup>& Groups) {
        // Process user's groups for display
    });
    
    // Fetch user groups
    GroupsSystem->FetchUserGroups(OnFetchUserGroups);
}
```

## Searching for Groups

You can search for groups by name:

```cpp
void UMyGameMode::SearchGroups(const FString& Query)
{
    UGameFuseGroups* GroupsSystem = GetGameInstance()->GetSubsystem<UGameFuseGroups>();
    
    // Create typed callback
    FGFGroupsCallback OnSearchGroups;
    OnSearchGroups.BindLambda([this, Query](const TArray<FGFGroup>& Groups) {
        // Process search results for display
    });
    
    // Search for groups
    GroupsSystem->SearchGroups(Query, OnSearchGroups);
}
```

## Managing Group Admins

You can add an admin to a group:

```cpp
void UMyGameMode::AddAdmin(int32 GroupId, int32 UserId)
{
    UGameFuseGroups* GroupsSystem = GetGameInstance()->GetSubsystem<UGameFuseGroups>();
    
    // Create typed callback
    FGFGroupActionCallback OnAddAdmin;
    OnAddAdmin.BindLambda([this, GroupId, UserId](bool bSuccess) {
        // Handle success or failure of adding admin
    });
    
    // Add admin
    GroupsSystem->AddAdmin(GroupId, UserId, OnAddAdmin);
}
```

And remove an admin:

```cpp
void UMyGameMode::RemoveAdmin(int32 GroupId, int32 UserId)
{
    UGameFuseGroups* GroupsSystem = GetGameInstance()->GetSubsystem<UGameFuseGroups>();
    
    // Create typed callback
    FGFGroupActionCallback OnRemoveAdmin;
    OnRemoveAdmin.BindLambda([this, GroupId, UserId](bool bSuccess) {
        // Handle success or failure of removing admin
    });
    
    // Remove admin
    GroupsSystem->RemoveAdmin(GroupId, UserId, OnRemoveAdmin);
}
```

## Managing Group Attributes

You can add an attribute to a group:

```cpp
void UMyGameMode::AddGroupAttribute(int32 GroupId, const FString& Key, const FString& Value)
{
    UGameFuseGroups* GroupsSystem = GetGameInstance()->GetSubsystem<UGameFuseGroups>();
    
    // Create typed callback
    FGFGroupActionCallback OnAddAttribute;
    OnAddAttribute.BindLambda([this, GroupId, Key, Value](bool bSuccess) {
        // Handle success or failure of adding attribute
    });
    
    // Add attribute
    GroupsSystem->AddAttribute(GroupId, Key, Value, OnAddAttribute);
}
```

You can update an existing attribute:

```cpp
void UMyGameMode::UpdateGroupAttribute(int32 GroupId, const FString& Key, const FString& Value)
{
    UGameFuseGroups* GroupsSystem = GetGameInstance()->GetSubsystem<UGameFuseGroups>();
    
    // Create typed callback
    FGFGroupActionCallback OnUpdateAttribute;
    OnUpdateAttribute.BindLambda([this, GroupId, Key, Value](bool bSuccess) {
        // Handle success or failure of updating attribute
    });
    
    // Update attribute
    GroupsSystem->UpdateAttribute(GroupId, Key, Value, OnUpdateAttribute);
}
```

And remove an attribute:

```cpp
void UMyGameMode::RemoveGroupAttribute(int32 GroupId, const FString& Key)
{
    UGameFuseGroups* GroupsSystem = GetGameInstance()->GetSubsystem<UGameFuseGroups>();
    
    // Create typed callback
    FGFGroupActionCallback OnRemoveAttribute;
    OnRemoveAttribute.BindLambda([this, GroupId, Key](bool bSuccess) {
        // Handle success or failure of removing attribute
    });
    
    // Remove attribute
    GroupsSystem->RemoveAttribute(GroupId, Key, OnRemoveAttribute);
}
```