# Forgot Password

You can implement this simple method in your app and we will handle all the
emails and password resets on our end.

Once you run this function, our system will send an email to that user if it
exists. The email will be branded like your app: it will have your app's name,
image logo and color so it will look cohesive. The sender's email is even
masked with your app's name.

The user will then reset their password online and then will be instructed that
they can login into your app.

!!! example
    ```cpp
    void UMyObject::ResetUserPassword(const FString& Email)
    {
        // Get the GameFuse Manager subsystem
        UGameFuseManager* GameFuseManager = GetGameInstance()->GetSubsystem<UGameFuseManager>();
        
        // Create a callback for the password reset operation
        FGFApiCallback CompletionCallback;
        CompletionCallback.AddLambda([this](const FGFAPIResponse& Response)
        {
            if(Response.bSuccess)
            {
                UE_LOG(LogTemp, Display, TEXT("Password reset email sent successfully"));
                UE_LOG(LogTemp, Display, TEXT("Response: %s"), *Response.ResponseStr);
            }
            else
            {
                UE_LOG(LogTemp, Error, TEXT("Failed to send password reset email"));
                UE_LOG(LogTemp, Error, TEXT("Response: %s"), *Response.ResponseStr);
            }
        });
        
        // Send password reset email
        GameFuseManager->SendPasswordResetEmail(Email, CompletionCallback);
    }
    ```

## Function Parameters

### `GameFuseManager->SendPasswordResetEmail`

| Parameter | Type | Description |
|-----------|------|-------------|
| `Email` | `FString` | The email address of the user requesting password reset |
| `Callback` | `FGFApiCallback` | Callback function to handle the response |

## Response Handling

The `FGFApiCallback` provides a generic response:
- `FGFAPIResponse Response`: Contains success status, response string, request ID, and response code

## Function return values

### `GameFuseManager->SendPasswordResetEmail`

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK - Password reset email sent successfully |
| `403`            | Invalid email address |
| `404`            | No user found with the specified email, or GameID or Token incorrect |
| `500`            | Unknown server error |

## User Experience

See <a href="/generic/forgot_password/#user-experience">Generic: Forgot password - User experience</a>.
