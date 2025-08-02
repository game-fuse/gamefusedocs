# Using the Store in Your Game

The GameFuse Store system allows you to create, manage, and purchase store items in your game. Store items are fetched when you call `FetchGameVariables()` and are refreshed every time you call it again.

## Getting Started with Store

To use the GameFuse Store system, you'll need to access the `UGameFuseManager` and `UGameFuseUser` subsystems:

```cpp
UGameFuseManager* GameFuseManager = GetGameInstance()->GetSubsystem<UGameFuseManager>();
UGameFuseUser* GameFuseUser = GetGameInstance()->GetSubsystem<UGameFuseUser>();
```

## Fetching Available Store Items

Store items are fetched when you call `FetchGameVariables()`. The items will be refreshed every time you call it again.

!!! example "C++ Example"
    ```cpp
    void UMyObject::FetchStoreItems()
    {
        // Get the GameFuse Manager subsystem
        UGameFuseManager* GameFuseManager = GetGameInstance()->GetSubsystem<UGameFuseManager>();
        
        // Create callback for the operation
        FGFApiCallback CompletionCallback;
        CompletionCallback.AddLambda([this, GameFuseManager](const FGFAPIResponse& Response)
        {
            if(Response.bSuccess)
            {
                UE_LOG(LogTemp, Display, TEXT("Store items fetched successfully"));
                
                // Get the store items from the manager
                const TArray<FGFStoreItem>& StoreItems = GameFuseManager->GetGameStoreItems();
                
                for (const FGFStoreItem& StoreItem : StoreItems)
                {
                    UE_LOG(LogTemp, Display, TEXT("Name: %s"), *StoreItem.Name);
                    UE_LOG(LogTemp, Display, TEXT("Category: %s"), *StoreItem.Category);
                    UE_LOG(LogTemp, Display, TEXT("ID: %d"), StoreItem.Id);
                    UE_LOG(LogTemp, Display, TEXT("Description: %s"), *StoreItem.Description);
                    UE_LOG(LogTemp, Display, TEXT("Cost: %d (credits)"), StoreItem.Cost);
                    UE_LOG(LogTemp, Display, TEXT("Icon URL: %s"), *StoreItem.IconUrl);
                }
            }
            else
            {
                UE_LOG(LogTemp, Error, TEXT("Failed to fetch store items: %s"), *Response.ResponseStr);
            }
        });
        
        // Fetch game variables (includes store items)
        GameFuseManager->FetchGameVariables(CompletionCallback);
    }
    ```



## Fetching Purchased Store Items

### Fetching Current User's Purchased Store Items

!!! example "C++ Example"
    ```cpp
    void UMyObject::FetchMyPurchasedStoreItems()
    {
        // Get the GameFuse User subsystem
        UGameFuseUser* GameFuseUser = GetGameInstance()->GetSubsystem<UGameFuseUser>();
        
        // Create callback for the operation
        FGFStoreItemsCallback CompletionCallback;
        CompletionCallback.BindLambda([this](bool bSuccess, const TArray<FGFStoreItem>& StoreItems)
        {
            if(bSuccess)
            {
                UE_LOG(LogTemp, Display, TEXT("Fetched %d purchased store items"), StoreItems.Num());
                
                for (const FGFStoreItem& StoreItem : StoreItems)
                {
                    UE_LOG(LogTemp, Display, TEXT("Owns Item: %s (ID: %d)"), *StoreItem.Name, StoreItem.Id);
                    UE_LOG(LogTemp, Display, TEXT("Category: %s"), *StoreItem.Category);
                    UE_LOG(LogTemp, Display, TEXT("Description: %s"), *StoreItem.Description);
                }
            }
            else
            {
                UE_LOG(LogTemp, Error, TEXT("Failed to fetch purchased store items"));
            }
        });
        
        // Fetch current user's purchased store items
        GameFuseUser->FetchMyPurchasedStoreItems(CompletionCallback);
    }
    ```



### Fetching Other Users' Purchased Store Items

!!! example "C++ Example"
    ```cpp
    void UMyObject::FetchUserPurchasedStoreItems(int32 UserId)
    {
        // Get the GameFuse User subsystem
        UGameFuseUser* GameFuseUser = GetGameInstance()->GetSubsystem<UGameFuseUser>();
        
        // Create callback for the operation
        FGFStoreItemsCallback CompletionCallback;
        CompletionCallback.BindLambda([this](bool bSuccess, const TArray<FGFStoreItem>& StoreItems)
        {
            if(bSuccess)
            {
                UE_LOG(LogTemp, Display, TEXT("Fetched %d purchased store items for user"), StoreItems.Num());
                
                for (const FGFStoreItem& StoreItem : StoreItems)
                {
                    UE_LOG(LogTemp, Display, TEXT("User's Purchased Item: %s (ID: %d)"), *StoreItem.Name, StoreItem.Id);
                    UE_LOG(LogTemp, Display, TEXT("Category: %s"), *StoreItem.Category);
                    UE_LOG(LogTemp, Display, TEXT("Description: %s"), *StoreItem.Description);
                }
            }
            else
            {
                UE_LOG(LogTemp, Error, TEXT("Failed to fetch user's purchased store items"));
            }
        });
        
        // Fetch specific user's purchased store items
        GameFuseUser->FetchUserPurchasedStoreItems(UserId, CompletionCallback);
    }
    ```


## Purchasing Store Items

To purchase a store item, you can use either the store item ID or the store item object. If the user doesn't have enough credits, the purchase will fail.

!!! example "C++ Example"
    ```cpp
    void UMyObject::PurchaseStoreItem(int32 StoreItemId)
    {
        // Get the GameFuse User subsystem
        UGameFuseUser* GameFuseUser = GetGameInstance()->GetSubsystem<UGameFuseUser>();
        
        // Create callback for the operation
        FGFStoreItemsCallback CompletionCallback;
        CompletionCallback.BindLambda([this](bool bSuccess, const TArray<FGFStoreItem>& StoreItems)
        {
            if(bSuccess)
            {
                UE_LOG(LogTemp, Display, TEXT("Store item purchased successfully"));
                UE_LOG(LogTemp, Display, TEXT("Updated purchased items count: %d"), StoreItems.Num());
                
                // The user's purchased store items list has been refreshed
                for (const FGFStoreItem& StoreItem : StoreItems)
                {
                    UE_LOG(LogTemp, Display, TEXT("Owned Item: %s (ID: %d)"), *StoreItem.Name, StoreItem.Id);
                }
            }
            else
            {
                UE_LOG(LogTemp, Error, TEXT("Failed to purchase store item"));
            }
        });
        
        // Purchase store item by ID
        GameFuseUser->PurchaseStoreItem(StoreItemId, CompletionCallback);
    }
    ```



## Removing Store Items

You can remove a store item from a user's inventory:

!!! example "C++ Example"
    ```cpp
    void UMyObject::RemoveStoreItem(int32 StoreItemId)
    {
        // Get the GameFuse User subsystem
        UGameFuseUser* GameFuseUser = GetGameInstance()->GetSubsystem<UGameFuseUser>();
        
        // Create callback for the operation
        FGFStoreItemsCallback CompletionCallback;
        CompletionCallback.BindLambda([this](bool bSuccess, const TArray<FGFStoreItem>& StoreItems)
        {
            if(bSuccess)
            {
                UE_LOG(LogTemp, Display, TEXT("Store item removed successfully"));
                UE_LOG(LogTemp, Display, TEXT("Updated purchased items count: %d"), StoreItems.Num());
            }
            else
            {
                UE_LOG(LogTemp, Error, TEXT("Failed to remove store item"));
            }
        });
        
        // Remove store item by ID
        GameFuseUser->RemoveStoreItem(StoreItemId, CompletionCallback);
    }
    ```



## Function Parameters

### Fetching Store Items

| Parameter | Type | Description |
|-----------|------|-------------|
| `UserId` | `int32` | The user ID to fetch purchased items for (optional, for user-specific fetch) |
| `Callback` | `FGFStoreItemsCallback` | Callback function to handle the response |

### Purchasing and Removing Store Items

| Parameter | Type | Description |
|-----------|------|-------------|
| `StoreItemId` | `int32` | The ID of the store item to purchase or remove |
| `Callback` | `FGFStoreItemsCallback` | Callback function to handle the response |

## Function Return Values

### `UGameFuseUser->PurchaseStoreItem`

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK - Item purchased successfully |
| `400`            | Bad request - Invalid parameters |
| `401`            | Unauthorized - User not signed in |
| `403`            | Not enough credits or item already purchased |
| `404`            | Item not found |
| `500`            | Unknown server error |

### `UGameFuseUser->RemoveStoreItem`

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK - Item removed successfully |
| `400`            | Bad request - Invalid parameters |
| `401`            | Unauthorized - User not signed in |
| `404`            | Item not found or not owned by user |
| `500`            | Unknown server error |

### `UGameFuseUser->FetchMyPurchasedStoreItems` / `UGameFuseUser->FetchUserPurchasedStoreItems`

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK - Purchased items fetched successfully |
| `401`            | Unauthorized - User not signed in |
| `404`            | User not found (for user-specific fetch) |
| `500`            | Unknown server error |

## Cached Data Access

You can access cached store data without making API calls:

```cpp
UGameFuseManager* GameFuseManager = GetGameInstance()->GetSubsystem<UGameFuseManager>();
UGameFuseUser* GameFuseUser = GetGameInstance()->GetSubsystem<UGameFuseUser>();

// Get cached available store items
const TArray<FGFStoreItem>& AvailableItems = GameFuseManager->GetGameStoreItems();

// Get cached purchased store items
const TArray<FGFStoreItem>& PurchasedItems = GameFuseUser->GetPurchasedStoreItems();
```
