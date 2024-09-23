# Custom user data

Custom user data or key-value pairs are a simple way to save any kind of data
for a specific user. An example might be:

```plaintext
{"world_2_unlocked":"true"} {"player_color","red"}, {"favorite_food","Onion"}
```

These are downloaded to your system upon login and synced when one is updated.

All values and keys must be strings. If you want to use other data structures
like arrays, you could stringify the array while saving. When loading the data
you must then convert the saved string into an array.

!!! example
    ```cpp
    void UMyObject::FetchAttributes()
    {
        FUserCallback CompletionCallback;
        CompletionCallback.BindDynamic(this, &UMyObject::OnAttributesFetchedCallback);

        UGameFuseUser* GameFuseUser = GetGameInstance()->GetSubsystem<UGameFuseUser> ();
        GameFuseUser->FetchAttributes(false, CompletionCallback);
    }

    void UMyObject::OnAttributesFetchedCallback(bool bSuccess, const FString& Response)
    {
        if(bSuccess)
        {
            UE_LOG(LogTemp, Display, TEXT("Game Connected Successfully"));
            UE_LOG(LogTemp, Display, TEXT("Result : %s"), *Response);

            UGameFuseUser* GameFuseUser = GetGameInstance()->GetSubsystem<UGameFuseUser> ();
            TMap Attributes = GameFuseUser->GetAttributes();

        }
        else
        {
            UE_LOG(LogTemp, Error, TEXT("Error connecting game"));
            UE_LOG(LogTemp, Error, TEXT("Result : %s"), *Response);
        }
    }

    void UMyObject::AddAttributes()
    {
        FUserCallback CompletionCallback;
        CompletionCallback.BindDynamic(this, &UMyObject::OnAttributesFetchedCallback);

        UGameFuseUser* GameFuseUser = GetGameInstance()->GetSubsystem<UGameFuseUser> ();
        GameFuseUser->SetAttribute("CURRENT_LEVEL", "5", CompletionCallback);
    }

    void UMyObject::OnAttributesAddedCallback(bool bSuccess, const FString& Response)
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

### `UGameFuseUser->SetAttribute`

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK |
| `400`            | Missing or invalid parameters, or some attribute is missing a `key` or `value` parameter |
| `500`            | Unknown server error |
