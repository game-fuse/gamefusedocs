# Custom User Data

Custom user data or key-value pairs are a simple way to save any kind of data for a specific user. An example might be:

```plaintext
{"world_2_unlocked":"true"} {"player_color","red"}, {"favorite_food","Onion"}
```

These are downloaded to your system upon login and synced when one is updated.

All values and keys must be strings. If you want to use other data structures like arrays, you could stringify the array while saving. When loading the data you must then convert the saved string into an array.

## Fetching Attributes

You can fetch the current user's attributes:

!!! example
    ```cpp
    void UMyObject::FetchAttributes()
    {
        // Get the GameFuse User subsystem
        UGameFuseUser* GameFuseUser = GetGameInstance()->GetSubsystem<UGameFuseUser>();
        
        // Create a typed callback for better type safety
        FGFAttributesCallback CompletionCallback;
        CompletionCallback.BindLambda([this](bool bSuccess, const FGFAttributeList& AttributeList)
        {
            if(bSuccess)
            {
                UE_LOG(LogTemp, Display, TEXT("Attributes fetched successfully"));
                
                // Access the attributes directly from the callback
                for (const auto& Attribute : AttributeList.Attributes)
                {
                    UE_LOG(LogTemp, Display, TEXT("Attribute: %s = %s"), *Attribute.Key, *Attribute.Value);
                }
            }
            else
            {
                UE_LOG(LogTemp, Error, TEXT("Failed to fetch attributes"));
            }
        });
        
        // Fetch user attributes
        GameFuseUser->FetchMyAttributes(CompletionCallback);
    }
    ```

## Setting Attributes

You can set a single attribute for the current user:

!!! example
    ```cpp
    void UMyObject::SetAttribute()
    {
        // Get the GameFuse User subsystem
        UGameFuseUser* GameFuseUser = GetGameInstance()->GetSubsystem<UGameFuseUser>();
        
        // Create a typed callback for better type safety
        FGFAttributesCallback CompletionCallback;
        CompletionCallback.BindLambda([this](bool bSuccess, const FGFAttributeList& AttributeList)
        {
            if(bSuccess)
            {
                UE_LOG(LogTemp, Display, TEXT("Attribute set successfully"));
                
                // Access the updated attributes
                for (const auto& Attribute : AttributeList.Attributes)
                {
                    UE_LOG(LogTemp, Display, TEXT("Updated Attribute: %s = %s"), *Attribute.Key, *Attribute.Value);
                }
            }
            else
            {
                UE_LOG(LogTemp, Error, TEXT("Failed to set attribute"));
            }
        });
        
        // Set a single attribute
        GameFuseUser->SetAttribute("CURRENT_LEVEL", "5", CompletionCallback);
    }
    ```

## Function Parameters

### Set Attribute

| Parameter | Type | Description |
|-----------|------|-------------|
| `Key` | `FString` | The key of the attribute |
| `Value` | `FString` | The value of the attribute |
| `Callback` | `FGFAttributesCallback` | Callback function to handle the response |

### Fetch My Attributes

| Parameter | Type | Description |
|-----------|------|-------------|
| `Callback` | `FGFAttributesCallback` | Callback function to handle the response |

## Function Return Values

### Set Attribute

| HTTP Status Code | Description |
|------------------|-------------|
| `200` | OK - Attribute set successfully |
| `400` | Missing or invalid parameters, or some attribute is missing a `key` or `value` parameter |
| `401` | User not authenticated |
| `500` | Unknown server error |
