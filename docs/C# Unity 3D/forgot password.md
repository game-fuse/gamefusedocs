# Forgot Password

You can implement this simple method in your app and we will handle all the
emails and password resets on our end.

Once you run this function, our system will send an email to that user if it
exists. The email will be branded like your app: it will have your app's name,
image logo and color so it will look cohesive. The sender's email is even
masked with your app's name.

The user will then reset their password online and then will be instructed that
they can login into your app.

## Sending Password Reset Email

Use `ForgotPasswordAsync` to initiate the password reset process for a user:

!!! example
    ```csharp
    async void SendPasswordResetEmail()
    {
        try
        {
            await GameFuseUser.ForgotPasswordAsync("john.doe@example.com");
            Debug.Log("Password reset email sent successfully");
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Error sending password reset email: {ex.Message}");
        }
    }
    ```

## With Custom Game Credentials

If you need to override the default game credentials from GameFuseSettings:

!!! example
    ```csharp
    async void SendPasswordResetEmailWithCustomCredentials()
    {
        try
        {
            await GameFuseUser.ForgotPasswordAsync(
                "john.doe@example.com",
                "your-game-id",
                "your-api-key");
                
            Debug.Log("Password reset email sent successfully");
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Error sending password reset email: {ex.Message}");
        }
    }
    ```

## Complete Example with UI Integration

Here's a comprehensive example showing how to integrate forgot password functionality with a UI:

!!! example
    ```csharp
    using UnityEngine;
    using UnityEngine.UI;
    using GameFuse;
    using System.Threading;
    
    public class ForgotPasswordUI : MonoBehaviour
    {
        [Header("UI References")]
        public InputField emailInputField;
        public Button sendResetButton;
        public Text statusText;
        public GameObject loadingIndicator;
        
        private CancellationTokenSource _cancellationTokenSource;
        
        void Start()
        {
            sendResetButton.onClick.AddListener(OnSendResetButtonClicked);
        }
        
        void OnDestroy()
        {
            _cancellationTokenSource?.Cancel();
            _cancellationTokenSource?.Dispose();
        }
        
        async void OnSendResetButtonClicked()
        {
            string email = emailInputField.text.Trim();
            
            if (string.IsNullOrEmpty(email))
            {
                ShowStatus("Please enter your email address", true);
                return;
            }
            
            if (!IsValidEmail(email))
            {
                ShowStatus("Please enter a valid email address", true);
                return;
            }
            
            await SendPasswordReset(email);
        }
        
        async System.Threading.Tasks.Task SendPasswordReset(string email)
        {
            try
            {
                // Cancel any previous operation
                _cancellationTokenSource?.Cancel();
                _cancellationTokenSource = new CancellationTokenSource();
                
                // Show loading state
                SetLoadingState(true);
                ShowStatus("Sending password reset email...", false);
                
                await GameFuseUser.ForgotPasswordAsync(email, cancellationToken: _cancellationTokenSource.Token);
                
                ShowStatus($"Password reset email sent to {email}. Please check your inbox.", false);
                
                // Optionally clear the input field
                emailInputField.text = "";
            }
            catch (System.OperationCanceledException)
            {
                ShowStatus("Operation cancelled", true);
            }
            catch (System.ArgumentException ex)
            {
                ShowStatus($"Invalid input: {ex.Message}", true);
            }
            catch (System.Exception ex)
            {
                ShowStatus($"Error: {ex.Message}", true);
            }
            finally
            {
                SetLoadingState(false);
            }
        }
        
        private void SetLoadingState(bool isLoading)
        {
            sendResetButton.interactable = !isLoading;
            loadingIndicator.SetActive(isLoading);
        }
        
        private void ShowStatus(string message, bool isError)
        {
            statusText.text = message;
            statusText.color = isError ? Color.red : Color.green;
        }
        
        private bool IsValidEmail(string email)
        {
            try
            {
                var emailAddress = new System.Net.Mail.MailAddress(email);
                return emailAddress.Address == email;
            }
            catch
            {
                return false;
            }
        }
    }
    ```

## Method Reference

### `ForgotPasswordAsync`
Initiates the forgot password process for a user.

**Parameters:**
- `email` (string): The email address of the user who forgot their password
- `gameId` (string, optional): The game ID. If not provided, uses value from GameFuseSettings
- `gameApiKey` (string, optional): The game API key. If not provided, uses value from GameFuseSettings
- `cancellationToken` (CancellationToken, optional): Token to cancel the operation

**Returns:** `Task` - Completes when the password reset email has been sent

**Usage Notes:**
- This is a static method that does not require an authenticated user
- The method will send a branded email if the user exists
- No sensitive information is revealed if the email doesn't exist (for security)
- The email contains a secure reset link that expires after a certain time
- Users complete the password reset process through the web interface

## Error Handling

The method may throw exceptions for various scenarios. Always wrap calls in try-catch blocks:

!!! example
    ```csharp
    async void SafeForgotPassword(string email)
    {
        try
        {
            await GameFuseUser.ForgotPasswordAsync(email);
            // Success - email sent
            Debug.Log("Reset email sent successfully");
        }
        catch (System.ArgumentNullException ex)
        {
            Debug.LogError($"Missing required parameter: {ex.ParamName}");
        }
        catch (System.ArgumentException ex)
        {
            Debug.LogError($"Invalid parameter: {ex.Message}");
        }
        catch (System.OperationCanceledException)
        {
            Debug.Log("Operation was cancelled");
        }
        catch (System.Exception ex)
        {
            Debug.LogError($"Unexpected error: {ex.Message}");
        }
    }
    ```

## Common Error Scenarios

- **Network connectivity issues** - User has no internet connection
- **Invalid email format** - Email address is malformed
- **Missing game credentials** - GameId or GameApiKey not configured properly
- **Server errors** - Temporary issues with the GameFuse service
- **Rate limiting** - Too many requests sent in a short time period

## Function return values

### HTTP Status Codes

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK - Password reset email sent successfully |
| `400`            | Bad Request - Invalid email format or missing parameters |
| `403`            | Forbidden - Invalid game credentials |
| `404`            | Not Found - No user found with the specified email, or GameID/Token incorrect |
| `429`            | Too Many Requests - Rate limiting applied |
| `500`            | Internal Server Error - Unknown server error |

## Security Notes

- The forgot password process is secure and does not reveal whether an email exists in the system
- Reset tokens have a limited lifespan and can only be used once
- The branded email helps prevent phishing attempts by maintaining your app's visual identity
- All password resets are logged for security auditing purposes