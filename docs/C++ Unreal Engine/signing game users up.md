# Sign game users up

Enable users to sign up in your UnrealEngine game with the following code.
These users will be saved in your GameFuse game and can then login from other
devices, since the data is saved online.

!!! example
    ```cpp
    void UMyObject::SignUp(const FString& Email, const FString& Password, const FString& PasswordConfirmation, const FString& Username)
    {
        // Get the GameFuse User subsystem
        UGameFuseUser* GameFuseUser = GetGameInstance()->GetSubsystem<UGameFuseUser>();
        
        // Create a typed callback for better type safety
        FGFUserDataCallback CompletionCallback;
        CompletionCallback.BindLambda([this](bool bSuccess, const FGFUserData& UserData)
        {
            if(bSuccess)
            {
                UE_LOG(LogTemp, Display, TEXT("User signed up successfully"));
                UE_LOG(LogTemp, Display, TEXT("Username: %s"), *UserData.Username);
                UE_LOG(LogTemp, Display, TEXT("User ID: %d"), UserData.Id);
            }
            else
            {
                UE_LOG(LogTemp, Error, TEXT("Sign up failed"));
            }
        });
        
        // Sign up the user
        GameFuseUser->SignUp(Email, Password, PasswordConfirmation, Username, CompletionCallback);
    }
    ```

To access the game user object from anywhere in the code, you can use the
subsystem:

```cpp
UGameFuseUser* GameFuseUser = GetGameInstance()->GetSubsystem<UGameFuseUser>();
```

## Function Parameters

### `GameFuseUser->SignUp`

| Parameter | Type | Description |
|-----------|------|-------------|
| `Email` | `FString` | The user's email address |
| `Password` | `FString` | The user's password |
| `PasswordConfirmation` | `FString` | Password confirmation (must match Password) |
| `Username` | `FString` | The desired username |
| `Callback` | `FGFUserDataCallback` | Callback function to handle the response |

## Response Handling

The `FGFUserDataCallback` provides direct access to the user data:
- `bool bSuccess`: Whether the sign up was successful
- `const FGFUserData& UserData`: The user data if successful

## Function return values

### `GameFuseUser->SignUp`

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK - User created successfully |
| `422`            | Validation error - Check email format, password requirements, or username availability |
| `500`            | Unknown server error |
