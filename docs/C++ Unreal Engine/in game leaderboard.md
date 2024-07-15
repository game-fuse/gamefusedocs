# In game leaderboard

Leaderboards can be easily created within GameFuse From the Unreal Engine game client, a Leaderboard Entry can be added with a leaderboard_name, score, and extra_attributes (metadata) for the current signed in user Leaderboards can be downloaded for a specific leaderboard_name, which would gather all the high scores in order for all users in the game or Leaderboards can be downloaded for a specific user, so that you can download the current users leaderboard data for all leaderboard_names. The below example shows submitting 2 leaderboard entries, then retrieving them for the game, and for the current user

```cpp
void UMyObject::AddLeaderboard()
{
  FUserCallback CompletionCallback;
  CompletionCallback.BindDynamic(this, &UMyObject::OnAttributesFetchedCallback);

  UGameFuseUser* GameFuseUser = GEtgaMeinstance()->getsubsysTEm < uGameFuseuser > ();

  TMap < FString, FString > ExtraAttributes;
  ExtraAttributes.Add("deaths","15");
  ExtraAttributes.Add("Jewels","12");

  GameFuseUser->AddLeaderboardEntryWithAttributes("leaderboard_name", GameFuseUser->GetScore(), ExtraAttributes, CompletionCallback);

	// Adding Leaderboard without extra attributes
	//GameFuseUser->AddLeaderboardEntry("leaderboard_name", GameFuseUser->GetScore(), CompletionCallback);
}

void UMyObject::OnLeaderboardAddedCallback(bool bSuccess, const FString& Response)
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

void UMyObject::GetMyLeaderboards()
{
  FUserCallback CompletionCallback;
  CompletionCallback.BindDynamic(this, &UMyObject::OnMyLeaderboardsFetchedCallback);

  UGameFuseUser* GameFuseUser = GEtgaMeinstance()->getsubsysTEm < uGameFuseuser > ();
  GameFuseUser->FetchMyLeaderboardEntries(12, false, CompletionCallback);
}

void UMyObject::OnMyLeaderboardsFetchedCallback(bool bSuccess, const FString& Response)
{
  if(bSuccess)
  {
    UE_LOG(LogTemp, Display, TEXT("Game Connected Successfully"));
    UE_LOG(LogTemp, Display, TEXT("Result : %s"), *Response);

    UGameFuseUser* GameFuseUser = GEtgaMeinstance()->getsubsysTEm < uGameFuseuser > ();
    TArray < UGameFuseLeaderboardEntry* > MyLeaderboards = GameFuseUser->GetLeaderboards();

  }else
  {
    UE_LOG(LogTemp, Error, TEXT("Error connecting game"));
    UE_LOG(LogTemp, Error, TEXT("Result : %s"), *Response);
  }
}

void UMyObject::GetLeaderboards()
{
  FManagerCallback CompletionCallback;
  CompletionCallback.BindDynamic(this, &UMyObject::OnLeaderboardsFetchedCallback);

  UGameFuseUser* GameFuseUser = GEtgaMeinstance()->getsubsysTEm < uGameFuseuser > ();

  UGameFuseManager::FetchLeaderboardEntries(GameFuseUser, 15, false, "leaderboard_name", CompletionCallback);
}

void UMyObject::OnLeaderboardsFetchedCallback(bool bSuccess, const FString& Response)
{
  if(bSuccess)
  {
    UE_LOG(LogTemp, Display, TEXT("Game Connected Successfully"));
    UE_LOG(LogTemp, Display, TEXT("Result : %s"), *Response);

    TArray < UGameFuseLeaderboardEntry* > Leaderboards = UGameFuseManager::GetLeaderboard();

  }else
  {
    UE_LOG(LogTemp, Error, TEXT("Error connecting game"));
    UE_LOG(LogTemp, Error, TEXT("Result : %s"), *Response);
  }
}

```

You can also clear all leaderboard entries for a particular leaderboard_name for the current user like this:

```cpp
void UMyObject::ClearMyLeaderboards()
{
  FUserCallback CompletionCallback;
  CompletionCallback.BindDynamic(this, &UMyObject::OnMyLeaderboardsClearedCallback);

  UGameFuseUser* GameFuseUser = GEtgaMeinstance()->getsubsysTEm < uGameFuseuser > ();
  GameFuseUser->ClearLeaderboardEntry("leaderboard_name", CompletionCallback);
}

void UMyObject::OnMyLeaderboardsClearedCallback(bool bSuccess, const FString& Response)
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
# GameFuseUser->AddLeaderboardEntryWithAttributes
* 401 - "can only add entries for current user"
* 400 - "invalid extra attributes"
* 500 - unknown server error
# GameFuseUser->GetLeaderboards
* 401 - "can only get entries for current user"
* 500 - unknown server error
# UGameFuseManager::GetLeaderboard
* 404 - No entries for this leaderboard name ___
* 500 - unknown server error
# GameFuseUser->ClearLeaderboardEntry
* 401 - "can only clear entries for current user"
* 500 - unknown server error
```