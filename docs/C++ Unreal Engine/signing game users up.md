# Signing game users up

Enable users to sign up in your UnrealEngine game with the following code. They will be saved on your GameFuse Game and can then login from other devices since the data is saved online.

```cpp
void UMyObject::SignUp(const FString& Email, const FString& Password, const FString& PasswordConfirmation,
  const FString& Username)
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
  }else
  {
    UE_LOG(LogTemp, Error, TEXT("Error connecting game"));
    UE_LOG(LogTemp, Error, TEXT("Result : %s"), *Response);
  }
}

```

To access the game user object form anywhere in the code, you can use the subsystem:

```cpp
UGameFuseUser* GameFuseUser = GetGameInstance()->GetSubsystem < UGameFuseUser > ();

```

```
* 404 - Failed to fetch game variables, check your token and id
* 500 - unknown server error
```