# Groups System

GameFuse provides a robust groups system that allows players to form communities within your game. The group functionality is accessible through the authenticated `GameFuseUser.CurrentUser` instance.

## Getting Started

All group methods require user authentication. Ensure you have a signed-in user before calling any group methods:

!!! example
    ```csharp
    if (GameFuseUser.CurrentUser == null)
    {
        Debug.LogError("User must be authenticated first");
        return;
    }
    ```

## Creating Groups

Use `CreateGroupAsync` to create a new group with the current user as admin:

!!! example
    ```csharp
    async void CreateGroup()
    {
        try
        {
            var payload = new CreateGroupPayload
            {
                Name = "My Awesome Team",
                GroupType = "default",
                MaxGroupSize = 10,
                CanAutoJoin = false,
                IsInviteOnly = true,
                Searchable = true,
                AdminsOnlyCanCreateAttributes = true
            };
            
            var group = await GameFuseUser.CurrentUser.CreateGroupAsync(payload);
            Debug.Log($"Group created successfully! Group ID: {group.Id}, Name: {group.Name}");
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Failed to create group: {ex.Message}");
        }
    }
    ```

## Fetching Groups

### Get All Groups

Use `FetchAllGroupsAsync` to retrieve a list of all available groups:

!!! example
    ```csharp
    async void LoadAllGroups()
    {
        try
        {
            var groups = await GameFuseUser.CurrentUser.FetchAllGroupsAsync();
            Debug.Log($"Found {groups.Count} groups");
            
            foreach (var group in groups)
            {
                Debug.Log($"Group: {group.Name} (ID: {group.Id})");
                Debug.Log($"Members: {group.MemberCount}/{group.MaxGroupSize}");
            }
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Failed to fetch groups: {ex.Message}");
        }
    }
    ```

### Get Group Details

Use `FetchGroupDetailsAsync` to get detailed information about a specific group:

!!! example
    ```csharp
    async void LoadGroupDetails(int groupId)
    {
        try
        {
            var group = await GameFuseUser.CurrentUser.FetchGroupDetailsAsync(groupId);
            
            Debug.Log($"Group: {group.Name}");
            Debug.Log($"Type: {group.GroupType}");
            Debug.Log($"Members: {group.MemberCount}/{group.MaxGroupSize}");
            Debug.Log($"Can Auto Join: {group.CanAutoJoin}");
            Debug.Log($"Invite Only: {group.IsInviteOnly}");
            Debug.Log($"Searchable: {group.Searchable}");
            
            // Check if current user is an admin
            bool isAdmin = group.Admins?.Any(admin => admin.Id == GameFuseUser.CurrentUser.Id) ?? false;
            Debug.Log($"Current user is admin: {isAdmin}");
            
            // List all members
            if (group.Members != null)
            {
                foreach (var member in group.Members)
                {
                    Debug.Log($"Member: {member.Username} (ID: {member.Id})");
                }
            }
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Failed to fetch group details: {ex.Message}");
        }
    }
    ```

## Joining Groups

### Send Join Request

Use `SendGroupConnectionRequestAsync` to request to join a group:

!!! example
    ```csharp
    async void RequestToJoinGroup(int groupId)
    {
        try
        {
            var response = await GameFuseUser.CurrentUser.SendGroupConnectionRequestAsync(groupId);
            Debug.Log($"Join request sent! Connection ID: {response.Id}");
            Debug.Log($"Status: {response.Status}");
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Failed to send join request: {ex.Message}");
        }
    }
    ```

### Accept Membership Requests (Admin Only)

Use `AcceptGroupMembershipRequestAsync` to accept pending join requests as an admin:

!!! example
    ```csharp
    async void AcceptJoinRequest(int groupConnectionId)
    {
        try
        {
            var response = await GameFuseUser.CurrentUser.AcceptGroupMembershipRequestAsync(groupConnectionId);
            Debug.Log($"Join request accepted! Connection ID: {response.Id}");
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Failed to accept join request: {ex.Message}");
        }
    }
    ```

### Decline Membership Requests (Admin Only)

Use `DeclineGroupMembershipRequestAsync` to decline pending join requests as an admin:

!!! example
    ```csharp
    async void DeclineJoinRequest(int groupConnectionId)
    {
        try
        {
            var response = await GameFuseUser.CurrentUser.DeclineGroupMembershipRequestAsync(groupConnectionId);
            Debug.Log($"Join request declined! Connection ID: {response.Id}");
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Failed to decline join request: {ex.Message}");
        }
    }
    ```

## Group Attributes

### Create Single Attribute

Use `CreateGroupAttributeAsync` to add a single custom attribute to a group:

!!! example
    ```csharp
    async void AddGroupAttribute(int groupId)
    {
        try
        {
            var attribute = await GameFuseUser.CurrentUser.CreateGroupAttributeAsync(
                groupId, 
                "score", 
                "1500", 
                false); // othersCanEdit = false
                
            Debug.Log($"Attribute created: {attribute.Key} = {attribute.Value}");
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Failed to create group attribute: {ex.Message}");
        }
    }
    ```

### Create Multiple Attributes

Use `CreateGroupAttributesAsync` to add multiple attributes at once:

!!! example
    ```csharp
    async void AddMultipleGroupAttributes(int groupId)
    {
        try
        {
            var attributesToCreate = new List<GroupAttributePayloadItem>
            {
                new GroupAttributePayloadItem { Key = "level", Value = "5", OthersCanEdit = false },
                new GroupAttributePayloadItem { Key = "rank", Value = "Gold", OthersCanEdit = true },
                new GroupAttributePayloadItem { Key = "region", Value = "North America", OthersCanEdit = false }
            };
            
            var response = await GameFuseUser.CurrentUser.CreateGroupAttributesAsync(groupId, attributesToCreate);
            Debug.Log($"Created {response.Attributes.Count} attributes");
            
            foreach (var attribute in response.Attributes)
            {
                Debug.Log($"Created: {attribute.Key} = {attribute.Value}");
            }
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Failed to create group attributes: {ex.Message}");
        }
    }
    ```

### Fetch Group Attributes

Use `FetchGroupAttributesAsync` to retrieve all attributes for a group:

!!! example
    ```csharp
    async void LoadGroupAttributes(int groupId)
    {
        try
        {
            var attributes = await GameFuseUser.CurrentUser.FetchGroupAttributesAsync(groupId);
            Debug.Log($"Group has {attributes.Count} attributes");
            
            foreach (var attribute in attributes)
            {
                Debug.Log($"{attribute.Key}: {attribute.Value}");
                Debug.Log($"  Others can edit: {attribute.OthersCanEdit}");
                Debug.Log($"  Created by: {attribute.CreatedByUsername}");
            }
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Failed to fetch group attributes: {ex.Message}");
        }
    }
    ```

### Modify Group Attributes

Use `ModifyGroupAttributeAsync` to update an existing group attribute:

!!! example
    ```csharp
    async void UpdateGroupAttribute(int groupId)
    {
        try
        {
            var payload = new ModifyGroupAttributePayload
            {
                Key = "score",
                Value = "2000"
            };
            
            var updatedAttribute = await GameFuseUser.CurrentUser.ModifyGroupAttributeAsync(groupId, payload);
            Debug.Log($"Updated attribute: {updatedAttribute.Key} = {updatedAttribute.Value}");
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Failed to modify group attribute: {ex.Message}");
        }
    }
    ```

## Complete Example

Here's a comprehensive example showing how to implement a group management system:

!!! example
    ```csharp
    using System.Collections.Generic;
    using UnityEngine;
    using UnityEngine.UI;
    using GameFuse;
    using GameFuse.Models.Shared;
    using System.Linq;
    
    public class GroupManager : MonoBehaviour
    {
        [Header("UI References")]
        public Transform groupsListParent;
        public GameObject groupItemPrefab;
        public Button createGroupButton;
        public InputField groupNameInput;
        public Toggle inviteOnlyToggle;
        public InputField maxSizeInput;
        
        [Header("Group Details")]
        public GameObject groupDetailsPanel;
        public Text groupNameText;
        public Text memberCountText;
        public Transform membersListParent;
        public Transform attributesListParent;
        public GameObject memberItemPrefab;
        public GameObject attributeItemPrefab;
        
        private Group currentGroup;
        
        void Start()
        {
            createGroupButton.onClick.AddListener(OnCreateGroupClicked);
            RefreshGroupsList();
        }
        
        async void OnCreateGroupClicked()
        {
            string groupName = groupNameInput.text.Trim();
            if (string.IsNullOrEmpty(groupName))
            {
                Debug.LogWarning("Please enter a group name");
                return;
            }
            
            await CreateNewGroup(groupName);
        }
        
        public async System.Threading.Tasks.Task CreateNewGroup(string name)
        {
            try
            {
                var payload = new CreateGroupPayload
                {
                    Name = name,
                    GroupType = "default",
                    MaxGroupSize = int.Parse(maxSizeInput.text),
                    CanAutoJoin = !inviteOnlyToggle.isOn,
                    IsInviteOnly = inviteOnlyToggle.isOn,
                    Searchable = true,
                    AdminsOnlyCanCreateAttributes = true
                };
                
                var group = await GameFuseUser.CurrentUser.CreateGroupAsync(payload);
                Debug.Log($"Group '{name}' created successfully!");
                
                // Clear input fields
                groupNameInput.text = "";
                maxSizeInput.text = "10";
                inviteOnlyToggle.isOn = false;
                
                await RefreshGroupsList();
            }
            catch (System.Exception ex)
            {
                Debug.LogError($"Error creating group: {ex.Message}");
            }
        }
        
        public async System.Threading.Tasks.Task RefreshGroupsList()
        {
            try
            {
                var groups = await GameFuseUser.CurrentUser.FetchAllGroupsAsync();
                
                // Clear existing items
                foreach (Transform child in groupsListParent)
                {
                    Destroy(child.gameObject);
                }
                
                // Create UI items for each group
                foreach (var group in groups)
                {
                    var groupItem = Instantiate(groupItemPrefab, groupsListParent);
                    var groupUI = groupItem.GetComponent<GroupItemUI>();
                    if (groupUI != null)
                    {
                        groupUI.Setup(group, this);
                    }
                }
            }
            catch (System.Exception ex)
            {
                Debug.LogError($"Error refreshing groups list: {ex.Message}");
            }
        }
        
        public async System.Threading.Tasks.Task ShowGroupDetails(int groupId)
        {
            try
            {
                currentGroup = await GameFuseUser.CurrentUser.FetchGroupDetailsAsync(groupId);
                
                // Update group info
                groupNameText.text = currentGroup.Name;
                memberCountText.text = $"Members: {currentGroup.MemberCount}/{currentGroup.MaxGroupSize}";
                
                // Update members list
                ClearParent(membersListParent);
                if (currentGroup.Members != null)
                {
                    foreach (var member in currentGroup.Members)
                    {
                        var memberItem = Instantiate(memberItemPrefab, membersListParent);
                        var memberUI = memberItem.GetComponent<MemberItemUI>();
                        if (memberUI != null)
                        {
                            bool isAdmin = currentGroup.Admins?.Any(admin => admin.Id == member.Id) ?? false;
                            memberUI.Setup(member, isAdmin);
                        }
                    }
                }
                
                // Load and display attributes
                await LoadGroupAttributes(groupId);
                
                groupDetailsPanel.SetActive(true);
            }
            catch (System.Exception ex)
            {
                Debug.LogError($"Error showing group details: {ex.Message}");
            }
        }
        
        async System.Threading.Tasks.Task LoadGroupAttributes(int groupId)
        {
            try
            {
                var attributes = await GameFuseUser.CurrentUser.FetchGroupAttributesAsync(groupId);
                
                ClearParent(attributesListParent);
                foreach (var attribute in attributes)
                {
                    var attributeItem = Instantiate(attributeItemPrefab, attributesListParent);
                    var attributeUI = attributeItem.GetComponent<AttributeItemUI>();
                    if (attributeUI != null)
                    {
                        attributeUI.Setup(attribute);
                    }
                }
            }
            catch (System.Exception ex)
            {
                Debug.LogError($"Error loading group attributes: {ex.Message}");
            }
        }
        
        public async System.Threading.Tasks.Task JoinGroup(int groupId)
        {
            try
            {
                var response = await GameFuseUser.CurrentUser.SendGroupConnectionRequestAsync(groupId);
                Debug.Log($"Join request sent for group {groupId}");
            }
            catch (System.Exception ex)
            {
                Debug.LogError($"Error joining group: {ex.Message}");
            }
        }
        
        public async System.Threading.Tasks.Task AddGroupAttribute(string key, string value, bool othersCanEdit)
        {
            if (currentGroup == null) return;
            
            try
            {
                var attribute = await GameFuseUser.CurrentUser.CreateGroupAttributeAsync(
                    currentGroup.Id, 
                    key, 
                    value, 
                    othersCanEdit);
                    
                Debug.Log($"Added attribute: {key} = {value}");
                await LoadGroupAttributes(currentGroup.Id);
            }
            catch (System.Exception ex)
            {
                Debug.LogError($"Error adding group attribute: {ex.Message}");
            }
        }
        
        void ClearParent(Transform parent)
        {
            foreach (Transform child in parent)
            {
                Destroy(child.gameObject);
            }
        }
    }
    
    // UI component classes would be similar to the friendship example
    public class GroupItemUI : MonoBehaviour
    {
        public Text groupNameText;
        public Text memberCountText;
        public Button viewButton;
        public Button joinButton;
        
        private GroupSummary group;
        private GroupManager manager;
        
        void Start()
        {
            viewButton.onClick.AddListener(() => manager.ShowGroupDetails(group.Id));
            joinButton.onClick.AddListener(() => manager.JoinGroup(group.Id));
        }
        
        public void Setup(GroupSummary groupData, GroupManager groupManager)
        {
            group = groupData;
            manager = groupManager;
            
            groupNameText.text = groupData.Name;
            memberCountText.text = $"{groupData.MemberCount}/{groupData.MaxGroupSize}";
        }
    }
    ```

## Method Reference

### `CreateGroupAsync`
Creates a new group with the current user as admin.

**Parameters:**
- `payload` (CreateGroupPayload): Group creation details including name, type, size limits, and permissions
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<Group>` - The newly created group

### `FetchAllGroupsAsync`
Fetches a list of all available groups (summary view).

**Parameters:**
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<IReadOnlyList<GroupSummary>>` - Read-only list of group summaries

### `FetchGroupDetailsAsync`
Fetches detailed information for a specific group.

**Parameters:**
- `groupId` (int): ID of the group to fetch
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<Group>` - Detailed group information including members and admins

### `SendGroupConnectionRequestAsync`
Sends a request to join a group.

**Parameters:**
- `groupId` (int): ID of the group to join
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<GroupConnectionResponse>` - Response with connection details

### `AcceptGroupMembershipRequestAsync`
Accepts a pending group membership request (admin only).

**Parameters:**
- `groupConnectionId` (int): ID of the connection request to accept
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<GroupConnectionStatusUpdateResponse>` - Updated connection status

### `DeclineGroupMembershipRequestAsync`
Declines a pending group membership request (admin only).

**Parameters:**
- `groupConnectionId` (int): ID of the connection request to decline
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<GroupConnectionStatusUpdateResponse>` - Updated connection status

### `CreateGroupAttributeAsync`
Creates a single custom attribute for a group.

**Parameters:**
- `groupId` (int): ID of the group
- `key` (string): Attribute key name
- `value` (string): Attribute value
- `othersCanEdit` (bool?, optional): Whether other members can edit this attribute
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<GroupAttributeResponseItem>` - The created attribute

### `CreateGroupAttributesAsync`
Creates multiple custom attributes for a group.

**Parameters:**
- `groupId` (int): ID of the group
- `attributesToCreate` (List<GroupAttributePayloadItem>): List of attributes to create
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<CreateGroupAttributesResponse>` - Response with created attributes

### `FetchGroupAttributesAsync`
Fetches all custom attributes for a group.

**Parameters:**
- `groupId` (int): ID of the group
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<IReadOnlyList<GroupAttributeResponseItem>>` - Read-only list of attributes

### `ModifyGroupAttributeAsync`
Modifies an existing group attribute.

**Parameters:**
- `groupId` (int): ID of the group
- `payload` (ModifyGroupAttributePayload): Details of the attribute to modify
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<GroupAttributeResponseItem>` - The updated attribute

## Error Handling

All group methods are async and may throw exceptions. Always wrap calls in try-catch blocks:

- **Network connectivity issues** - Connection problems
- **Authentication errors** - User not signed in
- **Permission errors** - Insufficient privileges for the action
- **Invalid parameters** - Group not found, invalid IDs
- **Group capacity** - Maximum group size reached
- **Server errors** - Temporary GameFuse service issues

## Function return values

### HTTP Status Codes

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK - Operation completed successfully |
| `400`            | Bad Request - Invalid parameters |
| `401`            | Unauthorized - User not authenticated |
| `403`            | Forbidden - Insufficient permissions |
| `404`            | Not Found - Group or resource not found |
| `409`            | Conflict - Group capacity reached or duplicate action |
| `500`            | Internal Server Error - Server error |