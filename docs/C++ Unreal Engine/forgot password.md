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
    void UMyObject::ResetUserPassword()
    {
        FManagerCallback CompletionCallback;
        CompletionCallback.BindDynamic(this, &UMyObject::OnMyLeaderboardsClearedCallback);

        UGameFuseManager::SendPasswordResetEmail("example@gmail.com", CompletionCallback);
    }

    void UMyObject::OnResetUserPasswordCallback(bool bSuccess, const FString& Response)
    {
        if(bSuccess)
        {
            UE_LOG(LogTemp, Display, TEXT("Game Connected Successfully"));
            UE_LOG(LogTemp, Display, TEXT("Result : %s"), *Response);
        }
        else
        {
          UE_LOG(LogTemp, Error, TEXT("Error connecting game"));
          UE_LOG(LogTemp, Error, TEXT("Result : %s"), *Response);
        }
    }
    ```

## Function return values

### `GameFuseUser.Instance.SendPasswordResetEmail`

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK |
| `403`            | Invalid email address |
| `404`            | No user found with the specified email, or GameID or Token incorrect |
| `500`            | Unknown server error |

## User Experience

See <a href="/generic/forgot_password/#user-experience">Generic: Forgot password - User experience</a>.
