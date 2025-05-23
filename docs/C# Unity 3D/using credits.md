# Using Credits

Credits are a fundamental currency system in GameFuse that allows players to purchase store items and participate in your game's economy. The GameFuse Unity SDK provides robust credit management through the `GameFuseUser` class with modern async/await methods for adding and setting credits.

## Overview

The GameFuse credit system provides:

- **Server-Side Validation**: All credit operations are validated and stored securely on the server
- **Automatic Synchronization**: Local user data is automatically updated after credit operations
- **Flexible Operations**: Both additive (`AddCreditsAsync`) and absolute (`SetCreditsAsync`) credit management
- **Store Integration**: Credits are automatically deducted during store purchases
- **Real-Time Updates**: Immediate reflection of credit changes in user properties

## Credit Properties

Credits are accessible through the authenticated user's properties:

```csharp
var currentUser = GameFuseUser.CurrentUser;
if (currentUser != null && currentUser.IsAuthenticated())
{
    Debug.Log($"Current Credits: {currentUser.Credits}");
}
```

## Adding Credits

### Basic Credit Addition

Use `AddCreditsAsync` to add (or subtract) credits from a user's current balance:

```csharp
using GameFuse;
using UnityEngine;
using System.Threading.Tasks;

public class CreditManager : MonoBehaviour
{
    public async void AddCreditsToUser(int amount)
    {
        try
        {
            if (GameFuseUser.CurrentUser == null)
            {
                Debug.LogError("No user signed in");
                return;
            }

            Debug.Log($"Current credits: {GameFuseUser.CurrentUser.Credits}");
            Debug.Log($"Adding {amount} credits...");

            // Add credits and get updated user data
            var updatedUser = await GameFuseUser.CurrentUser.AddCreditsAsync(amount);

            Debug.Log($"Credits added successfully!");
            Debug.Log($"New credit balance: {GameFuseUser.CurrentUser.Credits}");
        }
        catch (System.Exception e)
        {
            Debug.LogError($"Failed to add credits: {e.Message}");
        }
    }
}
```


## Setting Credits

### Absolute Credit Setting

Use `SetCreditsAsync` to set a user's credits to a specific absolute value:

```csharp
public async void SetUserCredits(int newAmount)
{
    try
    {
        if (GameFuseUser.CurrentUser == null)
        {
            Debug.LogError("No user signed in");
            return;
        }

        Debug.Log($"Current credits: {GameFuseUser.CurrentUser.Credits}");
        Debug.Log($"Setting credits to {newAmount}...");

        // Set credits to absolute value
        var updatedUser = await GameFuseUser.CurrentUser.SetCreditsAsync(newAmount);

        Debug.Log($"Credits set successfully!");
        Debug.Log($"New credit balance: {GameFuseUser.CurrentUser.Credits}");
    }
    catch (System.Exception e)
    {
        Debug.LogError($"Failed to set credits: {e.Message}");
    }
}
```

## Method Signatures

### AddCreditsAsync

```csharp
public async Task<User> AddCreditsAsync(
    int creditsAmount, 
    CancellationToken cancellationToken = default
)
```

**Parameters:**
- `creditsAmount` (int): Amount to add to current credits (can be negative for subtraction)
- `cancellationToken` (CancellationToken): Optional token to cancel the operation

**Returns:** `Task<User>` - The updated user data after the operation

### SetCreditsAsync

```csharp
public async Task<User> SetCreditsAsync(
    int newCredits, 
    CancellationToken cancellationToken = default
)
```

**Parameters:**
- `newCredits` (int): The new absolute credit amount for the user
- `cancellationToken` (CancellationToken): Optional token to cancel the operation

**Returns:** `Task<User>` - The updated user data after the operation

## Complete Credit Management Example

Here's a comprehensive example showing various credit operations with UI integration:

```csharp
using GameFuse;
using GameFuse.Models.Shared;
using UnityEngine;
using UnityEngine.UI;
using TMPro;
using System.Threading.Tasks;

public class CreditManagerUI : MonoBehaviour
{
    [Header("UI Elements")]
    public TextMeshProUGUI creditsDisplay;
    public TMP_InputField addAmountInput;
    public TMP_InputField setAmountInput;
    public Button addCreditsButton;
    public Button setCreditsButton;
    public Button rewardButton;
    public Button purchaseButton;
    public TextMeshProUGUI statusText;

    [Header("Preset Values")]
    public int dailyRewardAmount = 100;
    public int purchaseCost = 50;

    private void Start()
    {
        // Setup button listeners
        addCreditsButton.onClick.AddListener(() => HandleAddCredits());
        setCreditsButton.onClick.AddListener(() => HandleSetCredits());
        rewardButton.onClick.AddListener(() => HandleDailyReward());
        purchaseButton.onClick.AddListener(() => HandlePurchase());

        // Update display
        UpdateCreditsDisplay();
    }

    private void Update()
    {
        // Update credits display periodically
        UpdateCreditsDisplay();
    }

    public async void HandleAddCredits()
    {
        if (!ValidateUserSignedIn()) return;

        if (!int.TryParse(addAmountInput.text, out int amount))
        {
            ShowStatus("Please enter a valid number", false);
            return;
        }

        await AddCredits(amount);
    }

    public async void HandleSetCredits()
    {
        if (!ValidateUserSignedIn()) return;

        if (!int.TryParse(setAmountInput.text, out int amount) || amount < 0)
        {
            ShowStatus("Please enter a valid positive number", false);
            return;
        }

        await SetCredits(amount);
    }

    public async void HandleDailyReward()
    {
        if (!ValidateUserSignedIn()) return;
        await AddCredits(dailyRewardAmount, "Daily reward claimed!");
    }

    public async void HandlePurchase()
    {
        if (!ValidateUserSignedIn()) return;

        if (GameFuseUser.CurrentUser.Credits < purchaseCost)
        {
            ShowStatus($"Insufficient credits! Need {purchaseCost}, have {GameFuseUser.CurrentUser.Credits}", false);
            return;
        }

        await AddCredits(-purchaseCost, "Purchase completed!");
    }

    private async Task AddCredits(int amount, string successMessage = null)
    {
        try
        {
            SetUIEnabled(false);
            ShowStatus($"Adding {amount} credits...", true);

            var updatedUser = await GameFuseUser.CurrentUser.AddCreditsAsync(amount);

            string message = successMessage ?? $"Added {amount} credits successfully!";
            ShowStatus(message, true);
            
            Debug.Log($"Credits operation successful. New balance: {GameFuseUser.CurrentUser.Credits}");
        }
        catch (GameFuse.Exceptions.GameFuseApiException ex)
        {
            HandleCreditError(ex, "add credits");
        }
        catch (System.Exception e)
        {
            ShowStatus($"Unexpected error: {e.Message}", false);
        }
        finally
        {
            SetUIEnabled(true);
        }
    }

    private async Task SetCredits(int amount)
    {
        try
        {
            SetUIEnabled(false);
            ShowStatus($"Setting credits to {amount}...", true);

            var updatedUser = await GameFuseUser.CurrentUser.SetCreditsAsync(amount);

            ShowStatus($"Credits set to {amount} successfully!", true);
            
            Debug.Log($"Credits set successfully. New balance: {GameFuseUser.CurrentUser.Credits}");
        }
        catch (GameFuse.Exceptions.GameFuseApiException ex)
        {
            HandleCreditError(ex, "set credits");
        }
        catch (System.Exception e)
        {
            ShowStatus($"Unexpected error: {e.Message}", false);
        }
        finally
        {
            SetUIEnabled(true);
        }
    }

    private bool ValidateUserSignedIn()
    {
        if (GameFuseUser.CurrentUser == null || !GameFuseUser.CurrentUser.IsAuthenticated())
        {
            ShowStatus("Please sign in to manage credits", false);
            return false;
        }
        return true;
    }

    private void UpdateCreditsDisplay()
    {
        if (GameFuseUser.CurrentUser != null && GameFuseUser.CurrentUser.IsAuthenticated())
        {
            creditsDisplay.text = $"Credits: {GameFuseUser.CurrentUser.Credits}";
        }
        else
        {
            creditsDisplay.text = "Credits: Sign in required";
        }
    }

    private void HandleCreditError(GameFuse.Exceptions.GameFuseApiException ex, string operation)
    {
        switch (ex.StatusCode)
        {
            case System.Net.HttpStatusCode.BadRequest:
                ShowStatus($"Failed to {operation}: Invalid request", false);
                break;
            case System.Net.HttpStatusCode.Unauthorized:
                ShowStatus($"Failed to {operation}: Authentication required", false);
                break;
            default:
                ShowStatus($"Failed to {operation}: {ex.Message}", false);
                break;
        }
        Debug.LogError($"Credit operation failed: {ex.Message}");
    }

    private void ShowStatus(string message, bool isSuccess)
    {
        statusText.text = message;
        statusText.color = isSuccess ? Color.green : Color.red;
        
        // Clear status after 3 seconds
        CancelInvoke(nameof(ClearStatus));
        Invoke(nameof(ClearStatus), 3f);
    }

    private void ClearStatus()
    {
        statusText.text = "";
    }

    private void SetUIEnabled(bool enabled)
    {
        addCreditsButton.interactable = enabled;
        setCreditsButton.interactable = enabled;
        rewardButton.interactable = enabled;
        purchaseButton.interactable = enabled;
    }
}
```

## Game Integration Examples

### Daily Rewards System

```csharp
public class DailyRewardsManager : MonoBehaviour
{
    [Header("Reward Configuration")]
    public int[] dailyRewardAmounts = { 100, 150, 200, 250, 300, 400, 500 };
    
    private const string LAST_REWARD_KEY = "LastDailyReward";

    public async Task<bool> ClaimDailyReward()
    {
        // Check if reward is available
        if (!CanClaimDailyReward())
        {
            Debug.LogWarning("Daily reward not available yet");
            return false;
        }

        try
        {
            int dayIndex = GetCurrentRewardDay();
            int rewardAmount = dailyRewardAmounts[dayIndex % dailyRewardAmounts.Length];

            // Add credits for daily reward
            await GameFuseUser.CurrentUser.AddCreditsAsync(rewardAmount);

            // Save last reward time
            PlayerPrefs.SetString(LAST_REWARD_KEY, System.DateTime.Now.ToBinary().ToString());
            PlayerPrefs.Save();

            Debug.Log($"Daily reward claimed: {rewardAmount} credits");
            return true;
        }
        catch (System.Exception e)
        {
            Debug.LogError($"Failed to claim daily reward: {e.Message}");
            return false;
        }
    }

    private bool CanClaimDailyReward()
    {
        if (!PlayerPrefs.HasKey(LAST_REWARD_KEY)) return true;

        string lastRewardString = PlayerPrefs.GetString(LAST_REWARD_KEY);
        if (long.TryParse(lastRewardString, out long lastRewardBinary))
        {
            var lastReward = System.DateTime.FromBinary(lastRewardBinary);
            return (System.DateTime.Now - lastReward).TotalHours >= 24;
        }

        return true;
    }

    private int GetCurrentRewardDay()
    {
        if (!PlayerPrefs.HasKey(LAST_REWARD_KEY)) return 0;
        
        // Calculate consecutive days
        // Implementation depends on your specific requirements
        return PlayerPrefs.GetInt("ConsecutiveDays", 0);
    }
}
```

### Achievement Rewards

```csharp
public class AchievementSystem : MonoBehaviour
{
    [System.Serializable]
    public class Achievement
    {
        public string id;
        public string name;
        public string description;
        public int creditReward;
        public bool isCompleted;
    }

    [Header("Achievements")]
    public Achievement[] achievements;

    public async Task CompleteAchievement(string achievementId)
    {
        var achievement = System.Array.Find(achievements, a => a.id == achievementId);
        if (achievement == null || achievement.isCompleted)
        {
            return;
        }

        try
        {
            // Award credits for achievement
            await GameFuseUser.CurrentUser.AddCreditsAsync(achievement.creditReward);
            
            achievement.isCompleted = true;
            
            Debug.Log($"Achievement '{achievement.name}' completed! Awarded {achievement.creditReward} credits");
            
            // You might also want to save achievement progress
            await SaveAchievementProgress();
        }
        catch (System.Exception e)
        {
            Debug.LogError($"Failed to complete achievement: {e.Message}");
        }
    }

    private async Task SaveAchievementProgress()
    {
        // Save achievement completion status as user attributes
        foreach (var achievement in achievements)
        {
            if (achievement.isCompleted)
            {
                await GameFuseUser.CurrentUser.SetUserAttributeAsync(
                    $"achievement_{achievement.id}", 
                    "completed"
                );
            }
        }
    }
}
```

## Best Practices

### Credit Validation

```csharp
public class CreditValidator
{
    public static bool CanAfford(int cost)
    {
        var currentUser = GameFuseUser.CurrentUser;
        return currentUser != null && 
               currentUser.IsAuthenticated() && 
               currentUser.Credits >= cost;
    }

    public static async Task<bool> SafeSubtractCredits(int amount, string reason = "")
    {
        if (!CanAfford(amount))
        {
            Debug.LogWarning($"Cannot afford {amount} credits. Current: {GameFuseUser.CurrentUser?.Credits ?? 0}");
            return false;
        }

        try
        {
            await GameFuseUser.CurrentUser.AddCreditsAsync(-amount);
            Debug.Log($"Subtracted {amount} credits for: {reason}");
            return true;
        }
        catch (System.Exception e)
        {
            Debug.LogError($"Failed to subtract credits: {e.Message}");
            return false;
        }
    }
}
```

### Error Handling Patterns

```csharp
public enum CreditOperationResult
{
    Success,
    InsufficientCredits,
    NotAuthenticated,
    InvalidAmount,
    NetworkError
}

public static class CreditOperations
{
    public static async Task<CreditOperationResult> SafeAddCredits(int amount)
    {
        try
        {
            if (GameFuseUser.CurrentUser == null || !GameFuseUser.CurrentUser.IsAuthenticated())
            {
                return CreditOperationResult.NotAuthenticated;
            }

            if (amount == 0)
            {
                return CreditOperationResult.InvalidAmount;
            }

            await GameFuseUser.CurrentUser.AddCreditsAsync(amount);
            return CreditOperationResult.Success;
        }
        catch (GameFuse.Exceptions.GameFuseApiException ex)
        {
            Debug.LogError($"API Error: {ex.Message}");
            return CreditOperationResult.NetworkError;
        }
        catch (System.Exception e)
        {
            Debug.LogError($"Unexpected error: {e.Message}");
            return CreditOperationResult.NetworkError;
        }
    }
}
```

### Cancellation Support

```csharp
using System.Threading;

public class CancellableCreditOperations : MonoBehaviour
{
    private CancellationTokenSource _cancellationTokenSource;

    public async void StartLongRunningCreditOperation()
    {
        _cancellationTokenSource = new CancellationTokenSource();
        
        try
        {
            await GameFuseUser.CurrentUser.AddCreditsAsync(
                1000, 
                _cancellationTokenSource.Token
            );
            Debug.Log("Credit operation completed");
        }
        catch (OperationCanceledException)
        {
            Debug.Log("Credit operation was cancelled");
        }
        catch (System.Exception e)
        {
            Debug.LogError($"Credit operation failed: {e.Message}");
        }
    }

    public void CancelOperation()
    {
        _cancellationTokenSource?.Cancel();
    }

    private void OnDestroy()
    {
        _cancellationTokenSource?.Cancel();
        _cancellationTokenSource?.Dispose();
    }
}
```

## Integration with Store System

Credits work seamlessly with the GameFuse store system:

```csharp
public async void PurchaseItemWithCreditCheck(int storeItemId, int itemCost)
{
    // Check if user has enough credits
    if (!CreditValidator.CanAfford(itemCost))
    {
        Debug.LogWarning($"Insufficient credits for purchase. Need {itemCost}, have {GameFuseUser.CurrentUser.Credits}");
        
        // Offer to add credits
        await OfferCreditPurchase(itemCost - GameFuseUser.CurrentUser.Credits);
        return;
    }

    try
    {
        // Purchase the item (this will automatically deduct credits)
        var result = await GameFuseUser.CurrentUser.PurchaseStoreItemAsync(storeItemId);
        
        Debug.Log($"Item purchased! New credit balance: {result.Credits}");
    }
    catch (System.Exception e)
    {
        Debug.LogError($"Purchase failed: {e.Message}");
    }
}

private async Task OfferCreditPurchase(int neededCredits)
{
    // Add enough credits for the purchase plus some extra
    int creditsToAdd = neededCredits + 100;
    
    try
    {
        await GameFuseUser.CurrentUser.AddCreditsAsync(creditsToAdd);
        Debug.Log($"Added {creditsToAdd} credits to account");
    }
    catch (System.Exception e)
    {
        Debug.LogError($"Failed to add credits: {e.Message}");
    }
}
```

## Key Features Summary

- **Server-Side Security**: All credit operations are validated and stored securely on GameFuse servers
- **Automatic Synchronization**: Local user data is automatically updated after every operation
- **Flexible Operations**: Support for both additive and absolute credit management
- **Store Integration**: Seamless integration with GameFuse store purchase system
- **Modern Async/Await**: Non-blocking operations with cancellation token support
- **Comprehensive Error Handling**: Detailed exception information for different failure scenarios

## Requirements

- GameFuse Unity SDK installed
- User must be authenticated to perform credit operations
- Internet connection for API communication
- Unity 2021.3 or later (for async/await support)