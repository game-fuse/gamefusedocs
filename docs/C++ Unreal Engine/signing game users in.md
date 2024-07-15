# Signing game users in

Signing In Follows the same protocal as signing up, just with different parateters. As always, there is a callback function to let you know your sign in has been successful or not. Email and password (not username), will be used to sign in

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
  }else
  {
    UE_LOG(LogTemp, Error, TEXT("Error connecting game"));
    UE_LOG(LogTemp, Error, TEXT("Result : %s"), *Response);
  }
}

```

```
# 404 - Incorrect Password
# 404 - User Not Found!
# 402 - Game is disabled - developer check GameFuse dashboard
* 500 - unknown server error
```