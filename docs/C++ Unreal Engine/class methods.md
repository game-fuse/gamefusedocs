# Class Methods

Check each model below for a list of methods and attributes.

```cpp
// GameFuse Static Functions

UGameFuseManager::SetUpGame(const FString& InGameId, const FString& InToken, bool bSeedStore, FManagerCallback CompletionCallback);
UGameFuseManager::GetGameId();
UGameFuseManager::GetGameName();
UGameFuseManager::GetGameDescription();
UGameFuseManager::GetGameToken();
UGameFuseManager::GetBaseURL();
UGameFuseManager::GetGameVariables();
UGameFuseManager::GetGameStoreItems();
UGameFuseManager::GetLeaderboard();
UGameFuseManager::(const FString& Email, FManagerCallback CompletionCallback);
UGameFuseManager::FetchGameVariables(FManagerCallback CompletionCallback);
UGameFuseManager::FetchLeaderboardEntries(UGameFuseUser* GameFuseUser, const int Limit, bool bOnePerUser, const FString& LeaderboardName, FManagerCallback CompletionCallback);
UGameFuseManager::FetchStoreItems(FManagerCallback CompletionCallback);

// User Functions

UGameFuseUser* GameFuseUser = GEtgaMeinstance()->getsubsysTEm < uGameFuseuser > ();

GameFuseUser->GetNumberOfLogins();
GameFuseUser->GetLastLogin();
GameFuseUser->GetUsername();
GameFuseUser->GetScore();
GameFuseUser->GetCredits();
GameFuseUser->GetAttributes();
GameFuseUser->GetAttributesKeys();
GameFuseUser->GetAttributeValue(const FString Key);
GameFuseUser->GetPurchasedStoreItems();
GameFuseUser->GetLeaderboards();
GameFuseUser->SignUp(const FString& Email, const FString& Password, const FString& PasswordConfirmation, const FString& Username, FUserCallback CompletionCallback);
GameFuseUser->SignIn(const FString& Email, const FString& Password, FUserCallback CompletionCallback);
GameFuseUser->IsSignedIn() const;
GameFuseUser->AddCredits(const int AddCredits, FUserCallback CompletionCallback);
GameFuseUser->SetCredits(const int SetCredits, FUserCallback CompletionCallback);
GameFuseUser->AddScore(const int AddScore, FUserCallback CompletionCallback);
GameFuseUser->SetScore(const int SetScore, FUserCallback CompletionCallback);
GameFuseUser->SetAttribute(const FString& SetKey,const FString& SetValue, FUserCallback CompletionCallback);
GameFuseUser->RemoveAttribute(const FString& SetKey, FUserCallback CompletionCallback);
GameFuseUser->PurchaseStoreItemWithId(const int StoreItemId, FUserCallback CompletionCallback);
GameFuseUser->PurchaseStoreItem(const UGameFuseStoreItem* StoreItem, FUserCallback CompletionCallback);
GameFuseUser->RemoveStoreItemWithId(const int StoreItemId, FUserCallback CompletionCallback);
GameFuseUser->RemoveStoreItem(const UGameFuseStoreItem* StoreItem, FUserCallback CompletionCallback);
GameFuseUser->void AddLeaderboardEntry(const FString& LeaderboardName,const int OurScore, FUserCallback CompletionCallback);
GameFuseUser->AddLeaderboardEntryWithAttributes(const FString& LeaderboardName, const int OurScore, TMap ExtraAttributes, FUserCallback
        CompletionCallback);
GameFuseUser->ClearLeaderboardEntry(const FString& LeaderboardName, FUserCallback CompletionCallback);
GameFuseUser->FetchMyLeaderboardEntries(const int Limit, bool bOnePerUser, FUserCallback CompletionCallback);
GameFuseUser->FetchAttributes(bool bChainedFromLogin, FUserCallback CompletionCallback);
GameFuseUser->FetchPurchaseStoreItems(bool bChainedFromLogin, FUserCallback CompletionCallback);

// GameFuseStoreItem.h

TArray < UGameFuseStoreItem* > StoreItems = GameFuseUser->GetPurchasedStoreItems();

StoreItems->GetName();
StoreItems->GetCategory();
StoreItems->GetDescription();
StoreItems->GetCost();
StoreItems->GetId();
StoreItems->GetIconUrl();

// GameFuseLeaderboardEntry.h

TArray < UGameFuseLeaderboardEntry* > Leaderboards = UGameFuseManager::GetLeaderboard();

Leaderboards->GetUsername();
Leaderboards->GetScore();
Leaderboards->GetLeaderboardName();
Leaderboards->GetExtraAttributes();
Leaderboards->GetTimestamp();
```