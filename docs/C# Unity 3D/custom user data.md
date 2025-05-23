Custom user data or key-value pairs are a simple way to save any kind of data
for a specific user. An example might be:

```plaintext
{"world_2_unlocked":"true"} {"player_color","red"}, {"favorite_food","Onion"}
```

These are downloaded to your system upon login and synced when one is updated.

All values and keys must be strings. If you want to use other data structures
like arrays, you could stringify the array while saving. When loading the data
you must then convert the saved string into an array.

## Setting Single Attributes

Use `SetUserAttributeAsync` to set a single attribute for the current user:

!!! example
    ```csharp
    async void SetSingleAttribute()
    {
        try
        {
            var updatedUser = await GameFuseUser.CurrentUser.SetUserAttributeAsync("CURRENT_LEVEL", "5");
            Debug.Log($"Level set successfully: {updatedUser.Username}");
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Error setting attribute: {ex.Message}");
        }
    }
    ```

## Setting Multiple Attributes

Use `SetUserAttributesBatchAsync` to set multiple attributes at once:

!!! example
    ```csharp
    async void SetMultipleAttributes()
    {
        try
        {
            var attributesToSet = new List<KeyValuePair<string, string>>
            {
                new KeyValuePair<string, string>("POINTS", "1000"),
                new KeyValuePair<string, string>("LEVEL", "5"),
                new KeyValuePair<string, string>("CHARACTER", "Ninja")
            };
            
            var updatedUser = await GameFuseUser.CurrentUser.SetUserAttributesBatchAsync(attributesToSet);
            Debug.Log("Batch update complete");
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Error setting attributes: {ex.Message}");
        }
    }
    ```

## Getting User Attributes

Use `GetUserAttributesAsync` to retrieve all custom attributes for the current user or another user:

!!! example
    ```csharp
    async void GetCurrentUserAttributes()
    {
        try
        {
            var attributes = await GameFuseUser.CurrentUser.GetUserAttributesAsync();
            Debug.Log($"Found {attributes.Count} attributes");
            
            foreach (var attribute in attributes)
            {
                Debug.Log($"{attribute.Key}: {attribute.Value}");
            }
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Error getting attributes: {ex.Message}");
        }
    }
    
    async void GetOtherUserAttributes()
    {
        try
        {
            int otherUserId = 123;
            var attributes = await GameFuseUser.CurrentUser.GetUserAttributesAsync(otherUserId);
            Debug.Log($"User {otherUserId} has {attributes.Count} attributes");
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Error getting user attributes: {ex.Message}");
        }
    }
    ```

## Deleting Attributes

Use `DeleteUserAttributeAsync` to remove a specific attribute:

!!! example
    ```csharp
    async void DeleteAttribute()
    {
        try
        {
            var updatedUser = await GameFuseUser.CurrentUser.DeleteUserAttributeAsync("CURRENT_LEVEL");
            Debug.Log("Attribute deleted successfully");
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Error deleting attribute: {ex.Message}");
        }
    }
    ```

## Complete Example

Here's a comprehensive example showing all attribute operations:

!!! example
    ```csharp
    using System.Collections.Generic;
    using UnityEngine;
    using GameFuse;
    
    public class UserAttributeExample : MonoBehaviour
    {
        async void Start()
        {
            // Ensure user is authenticated first
            if (GameFuseUser.CurrentUser == null)
            {
                Debug.LogError("User must be authenticated first");
                return;
            }
            
            await DemonstrateAttributeOperations();
        }
        
        async System.Threading.Tasks.Task DemonstrateAttributeOperations()
        {
            try
            {
                // 1. Set a single attribute
                await GameFuseUser.CurrentUser.SetUserAttributeAsync("player_level", "10");
                Debug.Log("Single attribute set");
                
                // 2. Set multiple attributes
                var batchAttributes = new List<KeyValuePair<string, string>>
                {
                    new KeyValuePair<string, string>("coins", "500"),
                    new KeyValuePair<string, string>("gems", "25"),
                    new KeyValuePair<string, string>("last_world", "forest")
                };
                await GameFuseUser.CurrentUser.SetUserAttributesBatchAsync(batchAttributes);
                Debug.Log("Batch attributes set");
                
                // 3. Get all attributes
                var allAttributes = await GameFuseUser.CurrentUser.GetUserAttributesAsync();
                Debug.Log($"Retrieved {allAttributes.Count} attributes:");
                foreach (var attr in allAttributes)
                {
                    Debug.Log($"  {attr.Key} = {attr.Value}");
                }
                
                // 4. Delete an attribute
                await GameFuseUser.CurrentUser.DeleteUserAttributeAsync("last_world");
                Debug.Log("Attribute deleted");
                
                // 5. Verify deletion
                var updatedAttributes = await GameFuseUser.CurrentUser.GetUserAttributesAsync();
                Debug.Log($"After deletion: {updatedAttributes.Count} attributes remain");
            }
            catch (System.Exception ex)
            {
                Debug.LogError($"Error in attribute operations: {ex.Message}");
            }
        }
    }
    ```

## Method Reference

### `SetUserAttributeAsync`
Sets a single custom attribute for the current user.

**Parameters:**
- `key` (string): The attribute key
- `value` (string): The attribute value
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<User>` - The updated user data after refreshing

### `SetUserAttributesBatchAsync`
Sets multiple custom attributes for the current user in a single operation.

**Parameters:**
- `attributesToSet` (List<KeyValuePair<string, string>>): List of key-value pairs to set
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<User>` - The updated user data after refreshing

### `GetUserAttributesAsync`
Retrieves all custom attributes for the current user or a specified user.

**Parameters:**
- `userIdToFetch` (int, optional): The ID of the user whose attributes to fetch. If not provided, gets current user's attributes
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<UserAttributes>` - The user's custom attributes

### `DeleteUserAttributeAsync`
Deletes a specific user attribute by its key.

**Parameters:**
- `attributeKey` (string): The key of the attribute to delete
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task<User>` - The updated user data after refreshing

## Error Handling

All methods are async and may throw exceptions. Always wrap calls in try-catch blocks or handle exceptions appropriately. Common error scenarios include:

- Network connectivity issues
- Invalid authentication
- Server errors
- Invalid attribute keys or values

## Function return values

### HTTP Status Codes

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK |
| `400`            | Missing or invalid parameters, or some attribute is missing a `key` or `value` parameter |
| `401`            | Unauthorized - user not authenticated |
| `500`            | Unknown server error |