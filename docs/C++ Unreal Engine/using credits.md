# Using Credits

Credits are a numeric attribute of each game user. It is a simple integer value
that you can add manually.

Credits are automatically detracted upon store item purchases.

What you find in the example below is a script that demonstrates the full
lifecycle of credits of a signed-in user.  the `Start` function executes
these operations in order:

1. prints the credits your signed in user has
2. prints the cost of the first store item
3. adds credits to your user

Because the third operation syncs with the server it requires a callback.

Upon success you will see that the user now has more credits when logged. 

Finally you can run the *purchase store item* function successfully.

!!! example
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
        }
        else
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

        }
        else
        {
            UE_LOG(LogTemp, Error, TEXT("Error connecting game"));
            UE_LOG(LogTemp, Error, TEXT("Result : %s"), *Response);
        }
    }
    ```

## Function return values

### `UGameFuseUser->AddCredits`

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK |
| `400`            | Credits parameter missing |
| `500`            | Unknown server error |
