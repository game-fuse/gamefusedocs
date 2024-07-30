Signing in follows the same protocol as signing up, just with different
parateters.

There is a callback function to let you know your if sign-in has been
successfull or not. Email and password, not the username, will be used to
sign in.

!!! example
    ```cpp
    void UMyObject::SignIn(const FString& Email, const FString& Password)
    {
        FUserCallback CompletionCallback;
        CompletionCallback.BindDynamic(this, &UMyObject::SignedInCallback);

        UGameFuseUser* GameFuseUser = GEtgaMeinstance()->getsubsysTEm < uGameFuseuser > ();
        GameFuseUser->SignIn(Email, Password, CompletionCallback);
    }

    void UMyObject::SignedInCallback(bool bSuccess, const FString& Response)
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

### `GameFuse.SignUp`

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK |
| `404`            | Failed to fetch game variables. Check your token and Game ID |
| `500`            | Unknown server error |


```
# 404 - Incorrect Password
# 404 - User Not Found!
# 402 - Game is disabled - developer check GameFuse dashboard
* 500 - unknown server error
```
