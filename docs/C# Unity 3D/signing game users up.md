# Signing Game Users Up

The GameFuse Unity SDK provides a modern, async/await-based user registration system through the `GameFuseUser.SignUpAsync` method. This allows players to create accounts that are stored securely in the cloud and can be accessed from any device.

## Overview

User registration with GameFuse creates persistent player accounts with the following features:

- **Cloud Storage**: User data is stored securely online
- **Cross-Device Access**: Users can sign in from any device
- **Automatic Authentication**: Successfully registered users are automatically signed in
- **Integration Ready**: Immediate access to all GameFuse features after registration

## Basic Usage

### Simple Registration (Using GameFuseSettings)

If you've configured your `GameFuseSettings` ScriptableObject, registration is straightforward:

```csharp
using GameFuse;
using UnityEngine;
using System.Threading.Tasks;

public class UserRegistration : MonoBehaviour
{
    public async void RegisterUser(string email, string password, string username)
    {
        try
        {
            // GameFuseSettings automatically provides Game ID and API Key
            GameFuseUser newUser = await GameFuseUser.SignUpAsync(email, password, username);
            
            Debug.Log($"Successfully registered user: {newUser.Username}");
            Debug.Log($"User ID: {newUser.Id}");
            Debug.Log($"Email: {newUser.Email}");
            
            // User is now automatically signed in and available as CurrentUser
            Debug.Log($"Current user: {GameFuseUser.CurrentUser?.Username}");
        }
        catch (System.Exception e)
        {
            Debug.LogError($"Registration failed: {e.Message}");
        }
    }
}
```

### Registration with Custom Credentials

You can override the GameFuseSettings by providing explicit Game ID and API Key:

```csharp
public async void RegisterUserWithCustomCredentials(string email, string password, string username)
{
    try
    {
        GameFuseUser newUser = await GameFuseUser.SignUpAsync(
            email: email,
            password: password, 
            username: username,
            gameId: "your-custom-game-id",
            gameApiKey: "your-custom-api-key"
        );
        
        Debug.Log($"User registered with custom credentials: {newUser.Username}");
    }
    catch (System.Exception e)
    {
        Debug.LogError($"Registration failed: {e.Message}");
    }
}
```

## Method Signature

```csharp
public static async Task<GameFuseUser> SignUpAsync(
    string email, 
    string password, 
    string username, 
    string gameId = null, 
    string gameApiKey = null, 
    CancellationToken cancellationToken = default
)
```

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `email` | `string` | Yes | User's email address (used for sign-in) |
| `password` | `string` | Yes | User's password |
| `username` | `string` | Yes | Display name for the user |
| `gameId` | `string` | No* | Game ID from GameFuse dashboard |
| `gameApiKey` | `string` | No* | API Key from GameFuse dashboard |
| `cancellationToken` | `CancellationToken` | No | Token to cancel the operation |

**\*Note**: `gameId` and `gameApiKey` are automatically loaded from `GameFuseSettings` if not provided.

### Return Value

Returns a `Task<GameFuseUser>` representing the newly registered and authenticated user. The user is also automatically set as `GameFuseUser.CurrentUser`.

## Complete Registration Example

Here's a complete example showing a registration form with UI integration:

```csharp
using GameFuse;
using UnityEngine;
using UnityEngine.UI;
using TMPro;
using System.Threading.Tasks;

public class RegistrationManager : MonoBehaviour
{
    [Header("UI Elements")]
    public TMP_InputField emailInput;
    public TMP_InputField passwordInput;
    public TMP_InputField usernameInput;
    public Button registerButton;
    public TextMeshProUGUI statusText;

    private void Start()
    {
        registerButton.onClick.AddListener(() => HandleRegistration());
    }

    public async void HandleRegistration()
    {
        // Validate input
        if (string.IsNullOrEmpty(emailInput.text) || 
            string.IsNullOrEmpty(passwordInput.text) || 
            string.IsNullOrEmpty(usernameInput.text))
        {
            ShowStatus("Please fill in all fields", false);
            return;
        }

        // Disable UI during registration
        SetUIEnabled(false);
        ShowStatus("Creating account...", true);

        try
        {
            GameFuseUser newUser = await GameFuseUser.SignUpAsync(
                emailInput.text,
                passwordInput.text,
                usernameInput.text
            );

            ShowStatus($"Welcome, {newUser.Username}!", true);
            
            // Navigate to main game or update UI
            OnRegistrationSuccess(newUser);
        }
        catch (System.ArgumentNullException e)
        {
            ShowStatus("Configuration error: " + e.Message, false);
        }
        catch (GameFuse.Exceptions.GameFuseApiException e)
        {
            ShowStatus($"Registration failed: {e.Message}", false);
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

    private void OnRegistrationSuccess(GameFuseUser user)
    {
        // User is now registered and signed in
        // You can immediately access all GameFuse features
        Debug.Log($"User {user.Username} has {user.Credits} credits");
        
        // Navigate to main menu or game scene
        // SceneManager.LoadScene("MainMenu");
    }

    private void ShowStatus(string message, bool isSuccess)
    {
        statusText.text = message;
        statusText.color = isSuccess ? Color.green : Color.red;
    }

    private void SetUIEnabled(bool enabled)
    {
        registerButton.interactable = enabled;
        emailInput.interactable = enabled;
        passwordInput.interactable = enabled;
        usernameInput.interactable = enabled;
    }
}
```

## User Data Access

After successful registration, you can immediately access user information:

```csharp
// Access the newly registered user
GameFuseUser currentUser = GameFuseUser.CurrentUser;

// User properties
Debug.Log($"ID: {currentUser.Id}");
Debug.Log($"Username: {currentUser.Username}");
Debug.Log($"Email: {currentUser.Email}");
Debug.Log($"Credits: {currentUser.Credits}");
Debug.Log($"Score: {currentUser.Score}");
Debug.Log($"Login Count: {currentUser.NumberOfLogins}");
```

## Error Handling

### Common Exceptions

| Exception | Cause | Solution |
|-----------|-------|----------|
| `ArgumentNullException` | Missing Game ID or API Key | Configure GameFuseSettings or provide parameters |
| `GameFuseApiException` (404) | Invalid Game ID or API Key | Check credentials in GameFuse dashboard |
| `GameFuseApiException` (422) | Email already exists or validation error | Use different email or check input format |
| `GameFuseApiException` (402) | Game is disabled | Check GameFuse dashboard game status |

### Robust Error Handling Example

```csharp
public async Task<bool> SafeRegisterUser(string email, string password, string username)
{
    try
    {
        var user = await GameFuseUser.SignUpAsync(email, password, username);
        Debug.Log($"Registration successful: {user.Username}");
        return true;
    }
    catch (ArgumentNullException)
    {
        Debug.LogError("GameFuse not configured. Check GameFuseSettings.");
        return false;
    }
    catch (GameFuse.Exceptions.GameFuseApiException ex) when (ex.StatusCode == System.Net.HttpStatusCode.NotFound)
    {
        Debug.LogError("Invalid game credentials. Check Game ID and API Key.");
        return false;
    }
    catch (GameFuse.Exceptions.GameFuseApiException ex) when (ex.StatusCode == System.Net.HttpStatusCode.UnprocessableEntity)
    {
        Debug.LogError("Registration failed: Email may already be in use or input validation failed.");
        return false;
    }
    catch (GameFuse.Exceptions.GameFuseApiException ex) when (ex.StatusCode == System.Net.HttpStatusCode.PaymentRequired)
    {
        Debug.LogError("Game is disabled. Check GameFuse dashboard.");
        return false;
    }
    catch (System.Exception ex)
    {
        Debug.LogError($"Unexpected registration error: {ex.Message}");
        return false;
    }
}
```

## Best Practices

### Input Validation

```csharp
private bool ValidateRegistrationInput(string email, string password, string username)
{
    if (string.IsNullOrWhiteSpace(email) || !email.Contains("@"))
    {
        ShowError("Please enter a valid email address");
        return false;
    }

    if (string.IsNullOrWhiteSpace(password) || password.Length < 6)
    {
        ShowError("Password must be at least 6 characters");
        return false;
    }

    if (string.IsNullOrWhiteSpace(username) || username.Length < 3)
    {
        ShowError("Username must be at least 3 characters");
        return false;
    }

    return true;
}
```

### Cancellation Support

```csharp
using System.Threading;

private CancellationTokenSource _registrationCancellation;

public async void StartRegistration()
{
    _registrationCancellation = new CancellationTokenSource();
    
    try
    {
        var user = await GameFuseUser.SignUpAsync(
            email, password, username, 
            cancellationToken: _registrationCancellation.Token
        );
        // Handle success
    }
    catch (OperationCanceledException)
    {
        Debug.Log("Registration was cancelled");
    }
}

public void CancelRegistration()
{
    _registrationCancellation?.Cancel();
}
```

## Integration with Other Features

After successful registration, users automatically have access to all GameFuse features:

```csharp
private async void OnRegistrationComplete(GameFuseUser user)
{
    // User is signed in and ready to use all features
    
    // Set up initial user data
    await user.SetUserAttributeAsync("first_login", System.DateTime.Now.ToString());
    
    // Add welcome credits
    await user.AddCreditsAsync(100);
    
    // Check for available store items
    var storeItems = await user.GetAvailableStoreItemsAsync();
    
    Debug.Log($"User registered with {user.Credits} credits and access to {storeItems.Count} store items");
}
```

## Requirements

- GameFuse Unity SDK installed
- GameFuseSettings configured with valid Game ID and API Key
- Internet connection for API communication
- Unity 2021.3 or later (for async/await support)