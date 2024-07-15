# Using Credits

Credits are a numeric attribute of each game user. It is a simple integer value. You can add them manually and they are detracted automatically upon store item purchases Below is a script to demonstrate the full lifecycle of credits on a signed in user. First it prints the credits your signed in user has, then prints the cost of the first store item, then it adds credits to your user. Because this syncs with the server, it requires a callback. Upon success, you will see the user now has more credits when logged. At this point in time you can then run the purchase store item function successfully.

```cpp
void UMyObject::UsingCredits()
{
  FUserCallback CompletionCallback;
  CompletionCallback.BindDynamic(this, &UMyObject::OnCreditAddedCallback);

  UGameFuseUser* GameFuseUser = GEtgaMeinstance()->getsubsysTEm < uGameFuseuser > ();
  TArray < UGameFuseStoreItem* > StoreItems = UGameFuseManager::GetGameStoreItems();

  UE_LOG(LogTemp, Error, TEXT("current credits : %d"), GameFuseUser->GetCredits());
  UE_LOG(LogTemp, Error, TEXT("first store item price : %d"), (UGameFuseManager::GetGameStoreItems().Top()->GetCost()));

  GameFuseUser->AddCredits(50, CompletionCallback);
}

void UMyObject::OnCreditAddedCallback(bool bSuccess, const FString& Response)
{
  if(bSuccess)
  {
    UE_LOG(LogTemp, Display, TEXT("Game Connected Successfully"));
    UE_LOG(LogTemp, Display, TEXT("Result : %s"), *Response);

    FUserCallback CompletionCallback;
    CompletionCallback.BindDynamic(this, &UMyObject::OnPurchaseStoreItemCallback);

    UGameFuseUser* GameFuseUser = GEtgaMeinstance()->getsubsysTEm < uGameFuseuser > ();
    GameFuseUser->PurchaseStoreItem(UGameFuseManager::GetGameStoreItems().Top(), CompletionCallback);
  }else
  {
    UE_LOG(LogTemp, Error, TEXT("Error connecting game"));
    UE_LOG(LogTemp, Error, TEXT("Result : %s"), *Response);
  }
}

void UMyObject::OnPurchaseStoreItemCallback(bool bSuccess, const FString& Response)
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
* 400 - Please include a credits param
* 500 - unknown server error
```