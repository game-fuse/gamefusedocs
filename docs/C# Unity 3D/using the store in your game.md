# Using the Store in Your Game

The GameFuse Unity SDK provides a comprehensive in-game store system through the `GameFuseUser` class, allowing players to browse available items, make purchases with credits, and manage their inventory. The store system supports both authenticated and unauthenticated browsing, with purchasing requiring user authentication.

## Overview

The GameFuse store system includes:

- **Browse Available Items**: View all store items without requiring authentication
- **Purchase Management**: Buy and remove items with automatic credit balance updates
- **User Inventory**: Access purchased items and track ownership status
- **Real-time Updates**: Automatic synchronization of credits and inventory after transactions
- **Flexible Integration**: Works with GameFuseSettings or custom credentials

## Store Item Properties

Each store item contains the following information:

| Property | Type | Description |
|----------|------|-------------|
| `Id` | `int` | Unique identifier for the item |
| `Name` | `string` | Display name of the item |
| `Cost` | `int` | Price in credits |
| `Description` | `string` | Detailed description |
| `Category` | `string` | Item category for organization |
| `IconUrl` | `string` | URL to the item's icon image |
| `Status` | `StoreItemStatus` | Purchase status (Available, Purchased, Unavailable) |
| `IsPurchased` | `bool` | Quick check if item is owned |

## Getting Available Store Items

### Browse Store (No Authentication Required)

You can fetch all available store items without requiring user authentication:

```csharp
using GameFuse;
using UnityEngine;
using System.Threading.Tasks;

public class StoreManager : MonoBehaviour
{
    public async void LoadStoreItems()
    {
        try
        {
            // Uses GameFuseSettings for credentials
            var availableItems = await GameFuseUser.CurrentUser.GetAvailableStoreItemsAsync();
            
            Debug.Log($"Found {availableItems.Count} store items:");
            
            foreach (var item in availableItems)
            {
                Debug.Log($"Item: {item.Name}");
                Debug.Log($"  Cost: {item.Cost} credits");
                Debug.Log($"  Category: {item.Category}");
                Debug.Log($"  Description: {item.Description}");
                Debug.Log($"  Icon URL: {item.IconUrl}");
                Debug.Log($"  Status: {item.Status}");
            }
        }
        catch (System.Exception e)
        {
            Debug.LogError($"Failed to load store items: {e.Message}");
        }
    }
}
```

### Browse Store with Custom Credentials

```csharp
public async void LoadStoreItemsWithCustomCredentials()
{
    try
    {
        var availableItems = await GameFuseUser.CurrentUser.GetAvailableStoreItemsAsync(
            gameId: "your-game-id",
            gameToken: "your-api-key"
        );
        
        DisplayStoreItems(availableItems);
    }
    catch (System.Exception e)
    {
        Debug.LogError($"Failed to load store items: {e.Message}");
    }
}
```

## User Store Management

### Getting User's Purchased Items

Access the current user's purchased items and credit balance:

```csharp
public async void LoadUserStore()
{
    try
    {
        // Must be authenticated
        if (GameFuseUser.CurrentUser == null)
        {
            Debug.LogError("User must be signed in to access store");
            return;
        }

        UserStore userStore = await GameFuseUser.CurrentUser.GetUserStoreItemsAsync();
        
        Debug.Log($"User has {userStore.Credits} credits");
        Debug.Log($"User owns {userStore.StoreItems.Count} items:");
        
        foreach (var ownedItem in userStore.StoreItems)
        {
            Debug.Log($"  - {ownedItem.Name} (Cost: {ownedItem.Cost})");
        }
    }
    catch (System.Exception e)
    {
        Debug.LogError($"Failed to load user store: {e.Message}");
    }
}
```

### Accessing User Store from Properties

The current user's purchased items are also available directly from properties:

```csharp
public void ShowUserInventory()
{
    var currentUser = GameFuseUser.CurrentUser;
    if (currentUser == null)
    {
        Debug.LogError("No user signed in");
        return;
    }

    Debug.Log($"Credits: {currentUser.Credits}");
    Debug.Log($"Owned Items: {currentUser.GameUserStoreItems.Count}");
    
    foreach (var item in currentUser.GameUserStoreItems)
    {
        Debug.Log($"  - {item.Name}");
    }
}
```

## Purchasing Store Items

### Basic Purchase

Purchase an item and automatically update user credits and inventory:

```csharp
public async void PurchaseItem(int storeItemId)
{
    try
    {
        if (GameFuseUser.CurrentUser == null)
        {
            Debug.LogError("User must be signed in to purchase items");
            return;
        }

        Debug.Log($"Attempting to purchase item {storeItemId}...");
        
        UserStore result = await GameFuseUser.CurrentUser.PurchaseStoreItemAsync(storeItemId);
        
        if (result != null)
        {
            Debug.Log("Purchase successful!");
            Debug.Log($"New credit balance: {result.Credits}");
            Debug.Log($"Total owned items: {result.StoreItems.Count}");
            
            // User properties are automatically updated
            Debug.Log($"Current user credits: {GameFuseUser.CurrentUser.Credits}");
        }
        else
        {
            Debug.LogError("Purchase failed - no result returned");
        }
    }
    catch (GameFuse.Exceptions.GameFuseApiException ex)
    {
        HandlePurchaseError(ex);
    }
    catch (System.Exception e)
    {
        Debug.LogError($"Unexpected error during purchase: {e.Message}");
    }
}

private void HandlePurchaseError(GameFuse.Exceptions.GameFuseApiException ex)
{
    switch (ex.StatusCode)
    {
        case System.Net.HttpStatusCode.Forbidden:
            Debug.LogError("Purchase failed: Not enough credits or item already purchased");
            break;
        case System.Net.HttpStatusCode.NotFound:
            Debug.LogError("Purchase failed: Item not found");
            break;
        default:
            Debug.LogError($"Purchase failed: {ex.Message}");
            break;
    }
}
```

## Removing Store Items

### Remove Purchased Item

Remove an item from the user's inventory and get credits refunded:

```csharp
public async void RemoveItem(int storeItemId)
{
    try
    {
        if (GameFuseUser.CurrentUser == null)
        {
            Debug.LogError("User must be signed in to remove items");
            return;
        }

        Debug.Log($"Removing item {storeItemId}...");
        
        UserStore result = await GameFuseUser.CurrentUser.RemoveStoreItemAsync(storeItemId);
        
        if (result != null)
        {
            Debug.Log("Item removed successfully!");
            Debug.Log($"Credits refunded. New balance: {result.Credits}");
            Debug.Log($"Remaining owned items: {result.StoreItems.Count}");
        }
        else
        {
            Debug.LogError("Remove failed - no result returned");
        }
    }
    catch (System.Exception e)
    {
        Debug.LogError($"Failed to remove item: {e.Message}");
    }
}
```

## Complete Store UI Example

Here's a comprehensive example showing a complete store interface:

```csharp
using GameFuse;
using GameFuse.Models.Shared;
using UnityEngine;
using UnityEngine.UI;
using TMPro;
using System.Collections.Generic;
using System.Threading.Tasks;
using System.Linq;

public class GameStoreUI : MonoBehaviour
{
    [Header("UI Components")]
    public Transform storeItemContainer;
    public GameObject storeItemPrefab;
    public TextMeshProUGUI creditsText;
    public Button refreshButton;
    
    [Header("User Inventory")]
    public Transform inventoryContainer;
    public GameObject inventoryItemPrefab;
    
    private List<StoreItem> availableItems = new List<StoreItem>();
    private List<StoreItem> userItems = new List<StoreItem>();

    private void Start()
    {
        refreshButton.onClick.AddListener(() => RefreshStore());
        RefreshStore();
    }

    public async void RefreshStore()
    {
        await LoadAvailableItems();
        await LoadUserStore();
        UpdateUI();
    }

    private async Task LoadAvailableItems()
    {
        try
        {
            var items = await GameFuseUser.CurrentUser.GetAvailableStoreItemsAsync();
            availableItems = items.ToList();
            Debug.Log($"Loaded {availableItems.Count} available items");
        }
        catch (System.Exception e)
        {
            Debug.LogError($"Failed to load available items: {e.Message}");
        }
    }

    private async Task LoadUserStore()
    {
        try
        {
            if (GameFuseUser.CurrentUser != null && GameFuseUser.CurrentUser.IsAuthenticated())
            {
                var userStore = await GameFuseUser.CurrentUser.GetUserStoreItemsAsync();
                userItems = userStore.StoreItems.ToList();
                Debug.Log($"User owns {userItems.Count} items");
            }
        }
        catch (System.Exception e)
        {
            Debug.LogError($"Failed to load user store: {e.Message}");
        }
    }

    private void UpdateUI()
    {
        UpdateCreditsDisplay();
        UpdateStoreDisplay();
        UpdateInventoryDisplay();
    }

    private void UpdateCreditsDisplay()
    {
        if (GameFuseUser.CurrentUser != null && GameFuseUser.CurrentUser.IsAuthenticated())
        {
            creditsText.text = $"Credits: {GameFuseUser.CurrentUser.Credits}";
        }
        else
        {
            creditsText.text = "Credits: Sign in required";
        }
    }

    private void UpdateStoreDisplay()
    {
        // Clear existing items
        foreach (Transform child in storeItemContainer)
        {
            Destroy(child.gameObject);
        }

        // Create UI for each available item
        foreach (var item in availableItems)
        {
            GameObject itemUI = Instantiate(storeItemPrefab, storeItemContainer);
            SetupStoreItemUI(itemUI, item);
        }
    }

    private void UpdateInventoryDisplay()
    {
        // Clear existing items
        foreach (Transform child in inventoryContainer)
        {
            Destroy(child.gameObject);
        }

        // Create UI for each owned item
        foreach (var item in userItems)
        {
            GameObject itemUI = Instantiate(inventoryItemPrefab, inventoryContainer);
            SetupInventoryItemUI(itemUI, item);
        }
    }

    private void SetupStoreItemUI(GameObject itemUI, StoreItem item)
    {
        var nameText = itemUI.transform.Find("NameText").GetComponent<TextMeshProUGUI>();
        var costText = itemUI.transform.Find("CostText").GetComponent<TextMeshProUGUI>();
        var descriptionText = itemUI.transform.Find("DescriptionText").GetComponent<TextMeshProUGUI>();
        var buyButton = itemUI.transform.Find("BuyButton").GetComponent<Button>();

        nameText.text = item.Name;
        costText.text = $"{item.Cost} Credits";
        descriptionText.text = item.Description;

        // Check if user already owns this item
        bool isOwned = userItems.Any(owned => owned.Id == item.Id);
        bool canAfford = GameFuseUser.CurrentUser?.Credits >= item.Cost;
        bool isSignedIn = GameFuseUser.CurrentUser?.IsAuthenticated() == true;

        buyButton.interactable = isSignedIn && !isOwned && canAfford;
        buyButton.GetComponentInChildren<TextMeshProUGUI>().text = isOwned ? "Owned" : "Buy";

        buyButton.onClick.RemoveAllListeners();
        buyButton.onClick.AddListener(() => PurchaseItem(item.Id));
    }

    private void SetupInventoryItemUI(GameObject itemUI, StoreItem item)
    {
        var nameText = itemUI.transform.Find("NameText").GetComponent<TextMeshProUGUI>();
        var valueText = itemUI.transform.Find("ValueText").GetComponent<TextMeshProUGUI>();
        var removeButton = itemUI.transform.Find("RemoveButton").GetComponent<Button>();

        nameText.text = item.Name;
        valueText.text = $"Value: {item.Cost} Credits";

        removeButton.onClick.RemoveAllListeners();
        removeButton.onClick.AddListener(() => RemoveItem(item.Id));
    }

    public async void PurchaseItem(int itemId)
    {
        try
        {
            var result = await GameFuseUser.CurrentUser.PurchaseStoreItemAsync(itemId);
            
            if (result != null)
            {
                Debug.Log($"Purchase successful! New balance: {result.Credits}");
                await RefreshStore(); // Refresh to update UI
            }
        }
        catch (System.Exception e)
        {
            Debug.LogError($"Purchase failed: {e.Message}");
            // Show error message to user
        }
    }

    public async void RemoveItem(int itemId)
    {
        try
        {
            var result = await GameFuseUser.CurrentUser.RemoveStoreItemAsync(itemId);
            
            if (result != null)
            {
                Debug.Log($"Item removed! Credits refunded: {result.Credits}");
                await RefreshStore(); // Refresh to update UI
            }
        }
        catch (System.Exception e)
        {
            Debug.LogError($"Remove failed: {e.Message}");
            // Show error message to user
        }
    }
}
```

## Best Practices

### Cache Management

```csharp
public class StoreCache : MonoBehaviour
{
    private List<StoreItem> cachedItems;
    private float lastCacheTime;
    private const float CACHE_DURATION = 300f; // 5 minutes

    public async Task<List<StoreItem>> GetAvailableItems()
    {
        if (cachedItems == null || Time.time - lastCacheTime > CACHE_DURATION)
        {
            await RefreshCache();
        }
        return cachedItems;
    }

    private async Task RefreshCache()
    {
        try
        {
            var items = await GameFuseUser.CurrentUser.GetAvailableStoreItemsAsync();
            cachedItems = items.ToList();
            lastCacheTime = Time.time;
        }
        catch (System.Exception e)
        {
            Debug.LogError($"Failed to refresh store cache: {e.Message}");
        }
    }
}
```

### Purchase Validation

```csharp
public async Task<bool> CanPurchaseItem(StoreItem item)
{
    var currentUser = GameFuseUser.CurrentUser;
    
    // Check authentication
    if (currentUser == null || !currentUser.IsAuthenticated())
    {
        Debug.LogWarning("User must be signed in to purchase items");
        return false;
    }

    // Check credits
    if (currentUser.Credits < item.Cost)
    {
        Debug.LogWarning($"Insufficient credits. Need {item.Cost}, have {currentUser.Credits}");
        return false;
    }

    // Check if already owned
    if (currentUser.GameUserStoreItems.Any(owned => owned.Id == item.Id))
    {
        Debug.LogWarning("Item already owned");
        return false;
    }

    return true;
}
```

### Error Handling

```csharp
public enum PurchaseResult
{
    Success,
    InsufficientCredits,
    AlreadyOwned,
    ItemNotFound,
    NotAuthenticated,
    NetworkError
}

public async Task<PurchaseResult> SafePurchaseItem(int itemId)
{
    try
    {
        if (GameFuseUser.CurrentUser == null || !GameFuseUser.CurrentUser.IsAuthenticated())
        {
            return PurchaseResult.NotAuthenticated;
        }

        var result = await GameFuseUser.CurrentUser.PurchaseStoreItemAsync(itemId);
        
        if (result != null)
        {
            return PurchaseResult.Success;
        }
        
        return PurchaseResult.NetworkError;
    }
    catch (GameFuse.Exceptions.GameFuseApiException ex)
    {
        return ex.StatusCode switch
        {
            System.Net.HttpStatusCode.Forbidden => PurchaseResult.InsufficientCredits,
            System.Net.HttpStatusCode.NotFound => PurchaseResult.ItemNotFound,
            _ => PurchaseResult.NetworkError
        };
    }
    catch
    {
        return PurchaseResult.NetworkError;
    }
}
```

## Key Features Summary

- **No Authentication Browsing**: View store items without requiring user sign-in
- **Automatic Updates**: User credits and inventory are automatically synchronized after purchases
- **Real-time Validation**: Check purchase eligibility before attempting transactions
- **Flexible Configuration**: Use GameFuseSettings or provide custom credentials
- **Comprehensive Error Handling**: Detailed exception information for different failure scenarios
- **Modern Async/Await**: Non-blocking operations with cancellation token support

## Requirements

- GameFuse Unity SDK installed
- GameFuseSettings configured (for default credential usage)
- User authentication required for purchasing operations
- Internet connection for API communication