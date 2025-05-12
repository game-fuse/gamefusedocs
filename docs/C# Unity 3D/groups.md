# Groups System

GameFuse provides a robust groups system that allows players to form communities within your game. The `GameFuseUser.Groups` partial class contains methods to create and manage groups.

## Group System Methods

All group management methods are accessible through the `GameFuseUser` instance after a successful login.

### CreateGroupAsync

Creates a new group with the current user as admin.

**Parameters:**
- `CreateGroupRequest request` (required): Contains group creation parameters including:
  - `Name` (required): Name of the group
  - `GroupType`: Type of group (defaults to "default")
  - `MaxGroupSize` (required): Maximum number of members allowed
  - `CanAutoJoin` (required): Whether users can join without approval
  - `IsInviteOnly` (required): Whether group is invite-only
  - `Searchable`: Whether group appears in searches
  - `AdminsOnlyCanCreateAttributes`: Whether only admins can create attributes

**Returns:**
- `Task<GroupResponse>`: Response containing group details

**Example:**
```csharp
try
{
    // Get the current authenticated user
    GameFuseUser currentUser = GameFuseUser.CurrentUser;
    
    // Create group request
    CreateGroupRequest request = new CreateGroupRequest
    {
        Name = "My Awesome Team",
        MaxGroupSize = 10,
        CanAutoJoin = false,
        IsInviteOnly = true,
        Searchable = true,
        AdminsOnlyCanCreateAttributes = true
    };
    
    // Create the group
    GroupResponse response = await currentUser.CreateGroupAsync(request);
    
    // Access the group ID
    int groupId = response.Id;
    Debug.Log($"Group created successfully! Group ID: {groupId}");
}
catch (ApiException ex)
{
    Debug.LogError($"Failed to create group: {ex.Message}");
}
```

### GetAllGroupsAsync

Gets a list of all available groups.

**Parameters:**
- `bool withFullData = false` (optional): If true, includes complete group details including members and admins

**Returns:**
- `Task<GroupResponse[]>`: Array of GroupResponse objects containing group details

**Example:**
```csharp
try
{
    // Get the current authenticated user
    GameFuseUser currentUser = GameFuseUser.CurrentUser;
    
    // Get all groups with full details
    GroupResponse[] groups = await currentUser.GetAllGroupsAsync(true);
    
    // Display the groups
    foreach (GroupResponse group in groups)
    {
        Debug.Log($"Group: {group.Name}, Members: {group.MemberCount}/{group.MaxGroupSize}");
        
        // Access group members if needed
        foreach (UserInfo member in group.Members)
        {
            Debug.Log($"Member: {member.Username}");
        }
    }
}
catch (ApiException ex)
{
    Debug.LogError($"Failed to get groups: {ex.Message}");
}
```

### GetGroupDetailsAsync

Gets detailed information about a specific group.

**Parameters:**
- `int groupId` (required): ID of the group to fetch details for

**Returns:**
- `Task<GroupResponse>`: Response containing detailed group information

**Example:**
```csharp
try
{
    // Get the current authenticated user
    GameFuseUser currentUser = GameFuseUser.CurrentUser;
    
    // Get details for a specific group
    GroupResponse group = await currentUser.GetGroupDetailsAsync(12345);
    
    // Display group information
    Debug.Log($"Group: {group.Name}");
    Debug.Log($"Members: {group.MemberCount}/{group.MaxGroupSize}");
    Debug.Log($"Invite Only: {group.IsInviteOnly}");
    
    // Check if user is an admin
    bool isAdmin = group.Admins.Any(admin => admin.Id == currentUser.Id);
    Debug.Log($"Current user is admin: {isAdmin}");
}
catch (ApiException ex)
{
    Debug.LogError($"Failed to get group details: {ex.Message}");
}
```

### SendGroupConnectionRequestAsync

Sends a request to join a group or invites a user to join a group the current user administers.

**Parameters:**
- `GroupConnectionRequest request` (required): Group connection request parameters including:
  - `GroupId` (required): ID of the group
  - `UserId` (required): ID of the user

**Returns:**
- `Task<GroupConnectionResponse>`: Response containing connection details

**Example:**
```csharp
try
{
    // Get the current authenticated user
    GameFuseUser currentUser = GameFuseUser.CurrentUser;
    
    // Create connection request to join a group
    GroupConnectionRequest request = new GroupConnectionRequest
    {
        GroupId = 12345,
        UserId = currentUser.GetID()
    };
    
    // Send request to join
    GroupConnectionResponse response = await currentUser.SendGroupConnectionRequestAsync(request);
    
    Debug.Log($"Join request sent with connection ID: {response.Id}");
}
catch (ApiException ex)
{
    Debug.LogError($"Failed to send group connection request: {ex.Message}");
}
```

### AcceptGroupConnectionRequestAsync

Accepts a group connection request (join request or invitation).

**Parameters:**
- `int connectionId` (required): ID of the connection request to accept

**Returns:**
- `Task<GroupConnectionStatusResponse>`: Response confirming acceptance

**Example:**
```csharp
try
{
    // Get the current authenticated user
    GameFuseUser currentUser = GameFuseUser.CurrentUser;
    
    // Accept an invitation or join request
    GroupConnectionStatusResponse response = await currentUser.AcceptGroupConnectionRequestAsync(12345);
    
    Debug.Log("Group connection request accepted successfully!");
}
catch (ApiException ex)
{
    Debug.LogError($"Failed to accept group connection request: {ex.Message}");
}
```

### DeclineGroupConnectionRequestAsync

Declines a group connection request (join request or invitation).

**Parameters:**
- `int connectionId` (required): ID of the connection request to decline

**Returns:**
- `Task<GroupConnectionStatusResponse>`: Response confirming decline

**Example:**
```csharp
try
{
    // Get the current authenticated user
    GameFuseUser currentUser = GameFuseUser.CurrentUser;
    
    // Decline an invitation or join request
    GroupConnectionStatusResponse response = await currentUser.DeclineGroupConnectionRequestAsync(12345);
    
    Debug.Log("Group connection request declined successfully!");
}
catch (ApiException ex)
{
    Debug.LogError($"Failed to decline group connection request: {ex.Message}");
}
```

### GetGroupAttributesAsync

Gets all attributes for a specific group.

**Parameters:**
- `int groupId` (required): ID of the group to fetch attributes for

**Returns:**
- `Task<GroupAttributesResponse>`: Response containing group attributes

**Example:**
```csharp
try
{
    // Get the current authenticated user
    GameFuseUser currentUser = GameFuseUser.CurrentUser;
    
    // Get attributes for a group
    GroupAttributesResponse response = await currentUser.GetGroupAttributesAsync(12345);
    
    // Display group attributes
    foreach (GroupAttribute attribute in response.Attributes)
    {
        Debug.Log($"Attribute: {attribute.Key} = {attribute.Value}");
        Debug.Log($"Can be edited by others: {attribute.OthersCanEdit}");
    }
}
catch (ApiException ex)
{
    Debug.LogError($"Failed to get group attributes: {ex.Message}");
}
```

### AddGroupAttributeAsync

Adds a single attribute to a group.

**Parameters:**
- `int groupId` (required): ID of the group to add the attribute to
- `GroupAttributeRequest request` (required): Attribute request containing:
  - `Key` (required): Attribute key name
  - `Value` (required): Attribute value
  - `OthersCanEdit`: Whether non-creators can edit this attribute

**Returns:**
- `Task<GroupAttributesResponse>`: Response containing updated group attributes

**Example:**
```csharp
try
{
    // Get the current authenticated user
    GameFuseUser currentUser = GameFuseUser.CurrentUser;
    
    // Create attribute request
    GroupAttributeRequest request = new GroupAttributeRequest
    {
        Key = "score",
        Value = "1000",
        OthersCanEdit = false
    };
    
    // Add attribute to group
    GroupAttributesResponse response = await currentUser.AddGroupAttributeAsync(12345, request);
    
    Debug.Log("Group attribute added successfully!");
}
catch (ApiException ex)
{
    Debug.LogError($"Failed to add group attribute: {ex.Message}");
}
```

### AddGroupAttributesAsync

Adds multiple attributes to a group.

**Parameters:**
- `int groupId` (required): ID of the group to add attributes to
- `GroupAttributesRequest request` (required): Request containing an array of `GroupAttributeRequest` objects

**Returns:**
- `Task<GroupAttributesResponse>`: Response containing updated group attributes

**Example:**
```csharp
try
{
    // Get the current authenticated user
    GameFuseUser currentUser = GameFuseUser.CurrentUser;
    
    // Create attributes request
    GroupAttributesRequest request = new GroupAttributesRequest
    {
        Attributes = new[]
        {
            new GroupAttributeRequest { Key = "level", Value = "5", OthersCanEdit = false },
            new GroupAttributeRequest { Key = "rank", Value = "Gold", OthersCanEdit = true }
        }
    };
    
    // Add attributes to group
    GroupAttributesResponse response = await currentUser.AddGroupAttributesAsync(12345, request);
    
    Debug.Log($"Added {response.Attributes.Length} attributes to the group!");
}
catch (ApiException ex)
{
    Debug.LogError($"Failed to add group attributes: {ex.Message}");
}
```

### ModifyGroupAttributeAsync

Modifies an existing group attribute.

**Parameters:**
- `int groupId` (required): ID of the group containing the attribute
- `string key` (required): Key of the attribute to modify
- `string value` (required): New value for the attribute

**Returns:**
- `Task<GroupAttribute>`: Updated group attribute

**Example:**
```csharp
try
{
    // Get the current authenticated user
    GameFuseUser currentUser = GameFuseUser.CurrentUser;
    
    // Modify group attribute
    GroupAttribute updatedAttribute = await currentUser.ModifyGroupAttributeAsync(12345, "score", "2000");
    
    Debug.Log($"Updated attribute {updatedAttribute.Key} to {updatedAttribute.Value}");
}
catch (ApiException ex)
{
    Debug.LogError($"Failed to modify group attribute: {ex.Message}");
}
```

## Common Usage Pattern

Here's an example of how to implement a basic group manager in your game:

```csharp
using UnityEngine;
using GameFuseCSharp;
using System.Linq;
using UnityEngine.UI;

public class GroupManager : MonoBehaviour
{
    // UI elements for group list
    [SerializeField] private Transform groupListContainer;
    [SerializeField] private GameObject groupItemPrefab;
    
    // UI elements for group details
    [SerializeField] private GameObject groupDetailsPanel;
    [SerializeField] private Text groupNameText;
    [SerializeField] private Text memberCountText;
    [SerializeField] private Transform membersContainer;
    [SerializeField] private GameObject memberItemPrefab;
    
    private int currentGroupId = -1;
    
    // Load all available groups
    public async void LoadGroups()
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
            foreach (Transform child in groupListContainer)
            {
                Destroy(child.gameObject);
            }
            
            // Get all groups
            GroupResponse[] groups = await currentUser.GetAllGroupsAsync(false);
            
            // Create UI items for each group
            foreach (GroupResponse group in groups)
            {
                GameObject groupItem = Instantiate(groupItemPrefab, groupListContainer);
                GroupItemUI groupUI = groupItem.GetComponent<GroupItemUI>();
                groupUI.SetupGroup(group.Name, group.Id, group.MemberCount, group.MaxGroupSize);
                groupUI.OnGroupSelected += DisplayGroupDetails;
            }
        }
        catch (ApiException ex)
        {
            Debug.LogError($"Error loading groups: {ex.Message}");
        }
    }
    
    // Display details for a specific group
    public async void DisplayGroupDetails(int groupId)
    {
        try
        {
            GameFuseUser currentUser = GameFuseUser.CurrentUser;
            if (currentUser == null)
            {
                Debug.LogError("User not logged in");
                return;
            }
            
            currentGroupId = groupId;
            
            // Get detailed group information
            GroupResponse group = await currentUser.GetGroupDetailsAsync(groupId);
            
            // Update UI
            groupNameText.text = group.Name;
            memberCountText.text = $"Members: {group.MemberCount}/{group.MaxGroupSize}";
            
            // Clear existing member items
            foreach (Transform child in membersContainer)
            {
                Destroy(child.gameObject);
            }
            
            // Create UI items for each member
            foreach (UserInfo member in group.Members)
            {
                GameObject memberItem = Instantiate(memberItemPrefab, membersContainer);
                MemberItemUI memberUI = memberItem.GetComponent<MemberItemUI>();
                bool isAdmin = group.Admins.Any(admin => admin.Id == member.Id);
                memberUI.SetupMember(member.Username, member.Id, isAdmin);
            }
            
            // Show the details panel
            groupDetailsPanel.SetActive(true);
        }
        catch (ApiException ex)
        {
            Debug.LogError($"Error loading group details: {ex.Message}");
        }
    }
    
    // Create a new group
    public async void CreateGroup(string name, int maxSize, bool isInviteOnly, bool canAutoJoin)
    {
        try
        {
            GameFuseUser currentUser = GameFuseUser.CurrentUser;
            if (currentUser == null)
            {
                Debug.LogError("User not logged in");
                return;
            }
            
            CreateGroupRequest request = new CreateGroupRequest
            {
                Name = name,
                MaxGroupSize = maxSize,
                IsInviteOnly = isInviteOnly,
                CanAutoJoin = canAutoJoin,
                Searchable = true,
                AdminsOnlyCanCreateAttributes = true
            };
            
            GroupResponse response = await currentUser.CreateGroupAsync(request);
            Debug.Log($"Group '{name}' created successfully with ID: {response.Id}");
            
            // Refresh group list
            LoadGroups();
        }
        catch (ApiException ex)
        {
            Debug.LogError($"Error creating group: {ex.Message}");
        }
    }
    
    // Request to join a group
    public async void RequestToJoinGroup(int groupId)
    {
        try
        {
            GameFuseUser currentUser = GameFuseUser.CurrentUser;
            if (currentUser == null)
            {
                Debug.LogError("User not logged in");
                return;
            }
            
            GroupConnectionRequest request = new GroupConnectionRequest
            {
                GroupId = groupId,
                UserId = currentUser.GetID()
            };
            
            GroupConnectionResponse response = await currentUser.SendGroupConnectionRequestAsync(request);
            Debug.Log($"Join request sent for group {groupId}");
        }
        catch (ApiException ex)
        {
            Debug.LogError($"Error sending join request: {ex.Message}");
        }
    }
    
    // Add attribute to the current group
    public async void AddGroupAttribute(string key, string value, bool othersCanEdit)
    {
        if (currentGroupId < 0)
        {
            Debug.LogError("No group selected");
            return;
        }
        
        try
        {
            GameFuseUser currentUser = GameFuseUser.CurrentUser;
            if (currentUser == null)
            {
                Debug.LogError("User not logged in");
                return;
            }
            
            GroupAttributeRequest request = new GroupAttributeRequest
            {
                Key = key,
                Value = value,
                OthersCanEdit = othersCanEdit
            };
            
            await currentUser.AddGroupAttributeAsync(currentGroupId, request);
            Debug.Log($"Added attribute {key}={value} to group {currentGroupId}");
        }
        catch (ApiException ex)
        {
            Debug.LogError($"Error adding group attribute: {ex.Message}");
        }
    }
}

// UI component for displaying group members
public class MemberItemUI : MonoBehaviour
{
    [SerializeField] private Text usernameText;
    [SerializeField] private GameObject adminBadge;
    [SerializeField] private Button removeButton;
    
    private int userId;
    private string username;
    
    private void Start()
    {
        if (removeButton != null)
        {
            removeButton.onClick.AddListener(OnRemoveClicked);
        }
    }
    
    public void SetupMember(string username, int userId, bool isAdmin)
    {
        this.userId = userId;
        this.username = username;
        
        if (usernameText != null)
        {
            usernameText.text = username;
        }
        
        if (adminBadge != null)
        {
            adminBadge.SetActive(isAdmin);
        }
        
        // Only show remove button if the current user is an admin
        // This would need to be set based on your group details
        if (removeButton != null)
        {
            bool currentUserCanRemoveMembers = false; // Set this based on admin status
            removeButton.gameObject.SetActive(currentUserCanRemoveMembers);
        }
    }
    
    private void OnRemoveClicked()
    {
        // Implement removal logic here
        Debug.Log($"Request to remove user {username} (ID: {userId}) from group");
        
        // This would typically call a method on your GroupManager
        // For example: GroupManager.Instance.RemoveMemberFromGroup(userId);
    }
}

// UI component for displaying group items in a list
public class GroupItemUI : MonoBehaviour
{
    [SerializeField] private Text groupNameText;
    [SerializeField] private Text memberCountText;
    [SerializeField] private Button groupButton;
    
    private int groupId;
    private string groupName;
    
    // Event for when this group is selected
    public System.Action<int> OnGroupSelected;
    
    private void Start()
    {
        if (groupButton != null)
        {
            groupButton.onClick.AddListener(OnGroupClicked);
        }
    }
    
    public void SetupGroup(string name, int id, int memberCount, int maxSize)
    {
        this.groupId = id;
        this.groupName = name;
        
        if (groupNameText != null)
        {
            groupNameText.text = name;
        }
        
        if (memberCountText != null)
        {
            memberCountText.text = $"{memberCount}/{maxSize}";
        }
    }
    
    private void OnGroupClicked()
    {
        // Notify listeners that this group was selected
        OnGroupSelected?.Invoke(groupId);
    }
}
```
```

## Error Handling

All group-related methods throw an `ApiException` when a request fails. Always wrap your API calls in try-catch blocks to handle errors gracefully.

Common errors include:
- Insufficient permissions (trying to modify a group you don't administer)
- Maximum group size reached
- Group not found
- Attribute not found (when modifying)
- Network connectivity issues