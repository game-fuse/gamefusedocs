# Custom user data

Custom user data or Key Value pairs are a simple way to save any kind of data for a particular user. Some examples might be {"world_2_unlocked":"true"}, {"player_color","red"}, {"favorite_food","Onion"} These will fetched to your system after calling 'FetchAttributes()'.

All values and keys must be strings. If you want to use other data structures like arrays, you could stringify the array on save, and convert the saved string to an array on load:

```cpp
void UMyObject::FetchAttributes()
{
  FUserCallback CompletionCallback;
  CompletionCallback.BindDynamic(this, &UMyObject::OnAttributesFetchedCallback);

  UGameFuseUser* GameFuseUser = GEtgaMeinstance()->getsubsysTEm < uGameFuseuser > ();
  GameFuseUser->FetchAttributes(false, CompletionCallback);
}

void UMyObject::OnAttributesFetchedCallback(bool bSuccess, const FString& Response)
{
  if(bSuccess)
  {
    UE_LOG(LogTemp, Display, TEXT("Game Connected Successfully"));
    UE_LOG(LogTemp, Display, TEXT("Result : %s"), *Response);

    UGameFuseUser* GameFuseUser = GEtgaMeinstance()->getsubsysTEm < uGameFuseuser > ();
    TMap Attributes = GameFuseUser->GetAttributes();

  }else
  {
    UE_LOG(LogTemp, Error, TEXT("Error connecting game"));
    UE_LOG(LogTemp, Error, TEXT("Result : %s"), *Response);
  }
}

void UMyObject::AddAttributes()
{
  FUserCallback CompletionCallback;
  CompletionCallback.BindDynamic(this, &UMyObject::OnAttributesFetchedCallback);

  UGameFuseUser* GameFuseUser = GEtgaMeinstance()->getsubsysTEm < uGameFuseuser > ();
  GameFuseUser->SetAttribute("CURRENT_LEVEL", "5", CompletionCallback);
}

void UMyObject::OnAttributesAddedCallback(bool bSuccess, const FString& Response)
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
* 400 - each attribute a 'key' and 'value' parameter
* 400 - missing or invalid parameters
* 500 - unknown server error
```