# Sign game users in

Signing in follows the same protocol as signing up, just with different
parameters.

There is a callback function to let you know if your sign-in has been
successful or not. Email and password, not the username, will be used to
sign in.

!!! example
    ```cpp
    void UMyObject::SignIn(const FString& Email, const FString& Password)
    {
        // Get the GameFuse User subsystem
        UGameFuseUser* GameFuseUser = GetGameInstance()->GetSubsystem<UGameFuseUser>();
        
        // Create a typed callback for better type safety
        FGFUserDataCallback CompletionCallback;
        CompletionCallback.BindLambda([this](bool bSuccess, const FGFUserData& UserData)
        {
            if(bSuccess)
            {
                UE_LOG(LogTemp, Display, TEXT("User signed in successfully"));
                UE_LOG(LogTemp, Display, TEXT("Username: %s"), *UserData.Username);
                UE_LOG(LogTemp, Display, TEXT("User ID: %d"), UserData.Id);
                UE_LOG(LogTemp, Display, TEXT("Score: %d"), UserData.Score);
                UE_LOG(LogTemp, Display, TEXT("Credits: %d"), UserData.Credits);
            }
            else
            {
                UE_LOG(LogTemp, Error, TEXT("Sign in failed"));
            }
        });
        
        // Sign in the user
        GameFuseUser->SignIn(Email, Password, CompletionCallback);
    }
    ```

## Checking Sign In Status

You can check if a user is currently signed in:

```cpp
UGameFuseUser* GameFuseUser = GetGameInstance()->GetSubsystem<UGameFuseUser>();
if (GameFuseUser->IsSignedIn())
{
    UE_LOG(LogTemp, Display, TEXT("User is signed in"));
    FString Username = GameFuseUser->GetUsername();
    UE_LOG(LogTemp, Display, TEXT("Current user: %s"), *Username);
}
```

## Accessing User Data: Current vs. Last Fetched

After signing in, you can access user data using two functions:

- **GetCurrentUserData()**: Returns the currently signed-in user's data. This is updated after a successful sign-in or sign-up, and is what you should use for the local player.
- **GetLastFetchedUserData()**: Returns the most recently fetched user (for example, after calling `FetchUser(UserId, Callback)`). This is useful for viewing other users' profiles or data.

**Example:**
```cpp
UGameFuseUser* GameFuseUser = GetGameInstance()->GetSubsystem<UGameFuseUser>();
const FGFUserData& MyUser = GameFuseUser->GetCurrentUserData(); // Local signed-in user
const FGFUserData& OtherUser = GameFuseUser->GetLastFetchedUserData(); // Last user fetched by ID
```

- After sign-in, `GetCurrentUserData()` is updated with the signed-in user.
- After calling `FetchUser(UserId, Callback)`, `GetLastFetchedUserData()` is updated with that user's data.

## Function Parameters

### `GameFuseUser->SignIn`

| Parameter | Type | Description |
|-----------|------|-------------|
| `Email` | `FString` | The user's email address |
| `Password` | `FString` | The user's password |
| `Callback` | `FGFUserDataCallback` | Callback function to handle the response |

## Response Handling

The `FGFUserDataCallback` provides direct access to the user data:
- `bool bSuccess`: Whether the sign in was successful
- `const FGFUserData& UserData`: The user data if successful

## Function return values

### `GameFuseUser->SignIn`

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK - User signed in successfully |
| `401`            | Unauthorized - Incorrect email or password |
| `402`            | Game is disabled (check the GameFuse dashboard) |
| `404`            | User not found |
| `500`            | Unknown server error |

