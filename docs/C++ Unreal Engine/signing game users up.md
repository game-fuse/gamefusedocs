# Signing game users up

Enable users to sign up in your UnrealEngine game with the following code.
These users will be saved in your GameFuse game and can then login from other
devices, since the data is saved online.

!!! example
    ```cpp
    void UMyObject::SignUp(const FString& Email, const FString& Password, const FString& PasswordConfirmation, const FString& Username)
    {
        FUserCallback CompletionCallback;
        CompletionCallback.BindDynamic(this, &UMyObject::SignedUpCallback);

        UGameFuseUser* GameFuseUser = GEtgaMeinstance()->getsubsysTEm < uGameFuseuser > ();
        GameFuseUser->SignUp(Email, Password, PasswordConfirmation, Username, CompletionCallback);
    }

    void UMyObject::SignedUpCallback(bool bSuccess, const FString& Response)
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

To access the game user object form anywhere in the code, you can use the
subsystem:

```cpp
UGameFuseUser* GameFuseUser = GetGameInstance()->GetSubsystem < UGameFuseUser > ();
```

## Function return values

### `GameFuseUser->SignUp`

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK |
| `404`            | Failed to fetch game variables. Check your token and Game ID |
| `500`            | Unknown server error |
