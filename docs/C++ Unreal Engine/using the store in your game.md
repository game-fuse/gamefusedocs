# Using the store in your game

Store items are fetched upon `FetchGameVariables()`. The Items will
be refreshed every time you call 'FetchGameVariables()' again. To access store items and attributes by calling the following code. This will sync them with the available items on the server.

!!! example
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

            for (UGameFuseStoreItem* StoreItem : StoreItems)
            {
                if (StoreItem)
                {
                    UE_LOG(LogTemp, Display, TEXT("Name: %s"), *StoreItem->GetName());
                    UE_LOG(LogTemp, Display, TEXT("Category: %s"), *StoreItem->GetCategory());
                    UE_LOG(LogTemp, Display, TEXT("ID: %d"), StoreItem->GetId());
                    UE_LOG(LogTemp, Display, TEXT("Description: %s"), *StoreItem->GetDescription());
                    UE_LOG(LogTemp, Display, TEXT("Cost: %d (credits)"), StoreItem->GetCost());
                    UE_LOG(LogTemp, Display, TEXT("Icon URL: %s"), *StoreItem->GetIconUrl());
                }
            }
        }
        else
        {
            UE_LOG(LogTemp, Error, TEXT("Error connecting game"));
            UE_LOG(LogTemp, Error, TEXT("Result : %s"), *Response);
        }
    }
    ```

To access purchased store items by your current logged in user, call the
following code. Because these items are downloaded on login, there is no
callback for this: data is already available. This code will throw an error if
you are not signed in already.

!!! example
    ```cpp
    void UMyObject::FetchUserPurchasedStoreItems()
    {
        FUserCallback CompletionCallback;
        CompletionCallback.BindDynamic(this, &UMyObject::OnFetchUserPurchasedStoreItemsCallback);

        UGameFuseUser* GameFuseUser = GetGameInstance()->GetSubsystem<UGameFuseUser> ();
        GameFuseUser->FetchPurchaseStoreItems(false, CompletionCallback);
    }

    void UMyObject::OnFetchUserPurchasedStoreItemsCallback(bool bSuccess, const FString& Response)
    {
        if(bSuccess)
        {
            UE_LOG(LogTemp, Display, TEXT("Game Connected Successfully"));
            UE_LOG(LogTemp, Display, TEXT("Result : %s"), *Response);

            UGameFuseUser* GameFuseUser = GetGameInstance()->GetSubsystem<UGameFuseUser> ();
            TArray < UGameFuseStoreItem* > StoreItems = GameFuseUser->GetPurchasedStoreItems();
        }
        else
        {
            UE_LOG(LogTemp, Error, TEXT("Error connecting game"));
            UE_LOG(LogTemp, Error, TEXT("Result : %s"), *Response);
        }
    }
    ```

To purchase a store item simply call the code below. Because this function talks
to the server, it will require a callback. If the user does not have enough
credits on their account (see next section), the purchase will fail. This
function will refresh the `GameFuseUser.CurrentUser.purchasedStoreItems` list
with the new item.

!!! example
    ```cpp
    void UMyObject::PurchaseStoreItem(UGameFuseStoreItem* StoreItem)
    {
        FUserCallback CompletionCallback;
        CompletionCallback.BindDynamic(this, &UMyObject::OnFetchUserPurchasedStoreItemsCallback);

        UGameFuseUser* GameFuseUser = GetGameInstance()->GetSubsystem<UGameFuseUser> ();

        GameFuseUser->PurchaseStoreItem(StoreItem, CompletionCallback);

        // Purchase store item with id.
        //const int StoreItemID = < your-store-item-id > ;
        //GameFuseUser->PurchaseStoreItemWithId(StoreItemID, CompletionCallback);
    }

    void UMyObject::OnPurchaseStoreItemCallback(bool bSuccess, const FString& Response)
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

### `UGameFuseUser.PurchaseStoreItem`

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK |
| `403`            | Not enough credits or item already purchased |
| `404`            | Item not found |
| `500`            | Unknown server error |
