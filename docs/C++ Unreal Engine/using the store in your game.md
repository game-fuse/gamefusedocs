# Using the store in your game

Store Items Plugin will fetched upon 'FetchGameVariables()', The Items will be refreshed every time you call 'FetchGameVariables()' again. To access store items and attributes by calling the following code. This will sync them with the available items on the server.

```cpp
void UMyObject::FetchStoreItems()
{
  FManagerCallback CompletionCallback;
  CompletionCallback.BindDynamic(this, &UMyObject::SignedInCallback);

  UGameFuseManager::FetchGameVariables(CompletionCallback);
}

void UMyObject::OnStoreItemsFetchedCallback(bool bSuccess, const FString& Response)
{
  if(bSuccess)
  {
    UE_LOG(LogTemp, Display, TEXT("Game Connected Successfully"));
    UE_LOG(LogTemp, Display, TEXT("Result : %s"), *Response);

    TArray< UGameFuseStoreItem* > StoreItems = UGameFuseManager::GetGameStoreItems();

    for (UGameFuseStoreItem* StoreItem : StoreItems) {
      if (StoreItem) {
        UE_LOG(LogTemp, Display, TEXT("Name: %s"), *StoreItem->GetName());
        UE_LOG(LogTemp, Display, TEXT("Category: %s"), *StoreItem->GetCategory());
        UE_LOG(LogTemp, Display, TEXT("ID: %d"), StoreItem->GetId());
        UE_LOG(LogTemp, Display, TEXT("Description: %s"), *StoreItem->GetDescription());
        UE_LOG(LogTemp, Display, TEXT("Cost: %d (credits)"), StoreItem->GetCost());
        UE_LOG(LogTemp, Display, TEXT("Icon URL: %s"), *StoreItem->GetIconUrl());
      }
    }
  }else
  {
    UE_LOG(LogTemp, Error, TEXT("Error connecting game"));
    UE_LOG(LogTemp, Error, TEXT("Result : %s"), *Response);
  }
}

```

To access purchased store items by your current logged in user call the following. Because these are downloaded on login, there is no callback for this! It is already available! This will throw an error if you are not signed in already

```cpp
void UMyObject::FetchUserPurchasedStoreItems()
{
  FUserCallback CompletionCallback;
  CompletionCallback.BindDynamic(this, &UMyObject::OnFetchUserPurchasedStoreItemsCallback);

  UGameFuseUser* GameFuseUser = GEtgaMeinstance()->getsubsysTEm < uGameFuseuser > ();
  GameFuseUser->FetchPurchaseStoreItems(false, CompletionCallback);
}

void UMyObject::OnFetchUserPurchasedStoreItemsCallback(bool bSuccess, const FString& Response)
{
  if(bSuccess)
  {
    UE_LOG(LogTemp, Display, TEXT("Game Connected Successfully"));
    UE_LOG(LogTemp, Display, TEXT("Result : %s"), *Response);

    UGameFuseUser* GameFuseUser = GEtgaMeinstance()->getsubsysTEm < uGameFuseuser > ();
    TArray < UGameFuseStoreItem* > StoreItems = GameFuseUser->GetPurchasedStoreItems();

  }else
  {
    UE_LOG(LogTemp, Error, TEXT("Error connecting game"));
    UE_LOG(LogTemp, Error, TEXT("Result : %s"), *Response);
  }
}

```

To Purchase a store item simply call the code below. Because this function talks to the server, it will require a callback. If the user doesnt have enough credits on their account (see next section), the purchase will fail This function will refresh the GameFuseUser.CurrentUser.purchasedStoreItems List with the new item

```cpp
void UMyObject::PurchaseStoreItem(UGameFuseStoreItem* StoreItem)
{
  FUserCallback CompletionCallback;
  CompletionCallback.BindDynamic(this, &UMyObject::OnFetchUserPurchasedStoreItemsCallback);

  UGameFuseUser* GameFuseUser = GEtgaMeinstance()->getsubsysTEm < uGameFuseuser > ();

  GameFuseUser->PurchaseStoreItem(StoreItem, CompletionCallback);

	// Purchase Store Item With Id
	// const int StoreItemID = < your-store-item-id > ;
	// GameFuseUser->PurchaseStoreItemWithId(StoreItemID, CompletionCallback);
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
* 500 - unknown server error
```