# Class Methods

Check each model below for a list of methods and attributes.

## GameFuseManager

### C++ Functions

#### Getters
```cpp
// Functions that just return internal data
UGameFuseManager::GetGameData();
UGameFuseManager::GetGameId();
UGameFuseManager::GetGameName();
UGameFuseManager::GetGameDescription();
UGameFuseManager::GetGameToken();
UGameFuseManager::GetGameVariables();
UGameFuseManager::GetGameStoreItems();
UGameFuseManager::GetLeaderboards();
UGameFuseManager::GetLeaderboardEntries(const FString& LeaderboardName);
UGameFuseManager::IsSetUp();
UGameFuseManager::SetupCheck();
```

#### Operations
```cpp
// Functions that perform API operations
UGameFuseManager::ClearGameData();
UGameFuseManager::SetUpGame(const int GameId, const FString& Token, FGFApiCallback Callback);
UGameFuseManager::SendPasswordResetEmail(const FString& Email, FGFApiCallback Callback);
UGameFuseManager::FetchGameVariables(FGFApiCallback Callback);
UGameFuseManager::FetchLeaderboardEntries(const int Limit, bool bOnePerUser, const FString& LeaderboardName, FGFApiCallback Callback);
UGameFuseManager::FetchStoreItems(FGFApiCallback Callback);
UGameFuseManager::FetchServerTime(FGFApiCallback Callback);
```

### Blueprint Functions

#### Operations
```cpp
// Blueprint-specific functions
UGameFuseManager::BP_SetUpGame(const FString& GameId, const FString& Token, const FBP_GFApiCallback& Callback);
UGameFuseManager::BP_SendPasswordResetEmail(const FString& Email, const FBP_GFApiCallback& Callback);
UGameFuseManager::BP_FetchGameVariables(const FBP_GFApiCallback& Callback);
UGameFuseManager::BP_FetchLeaderboardEntries(const int Limit, bool bOnePerUser, const FString& LeaderboardName, const FBP_GFApiCallback& Callback);
UGameFuseManager::BP_FetchStoreItems(const FBP_GFApiCallback& Callback);
UGameFuseManager::BP_FetchServerTime(const FBP_GFApiCallback& Callback);
```

## GameFuseUser

### C++ Functions

#### Core User Data & Authentication

```cpp
// Getters
UGameFuseUser::GetCurrentUserData();
UGameFuseUser::GetLastFetchedUserData();
UGameFuseUser::GetUsername();
UGameFuseUser::GetNumberOfLogins();
UGameFuseUser::GetLastLogin();
UGameFuseUser::IsSignedIn();
UGameFuseUser::GetAuthenticationToken();

// Operations
UGameFuseUser::SignUp(const FString& Email, const FString& Password, const FString& PasswordConfirmation, const FString& Username, FGFUserDataCallback TypedCallback);
UGameFuseUser::SignUp(const FGFGameData& GameData, const FString& Email, const FString& Password, const FString& PasswordConfirmation, const FString& Username, FGFUserDataCallback TypedCallback);
UGameFuseUser::SignIn(const FString& Email, const FString& Password, FGFUserDataCallback TypedCallback);
UGameFuseUser::SignIn(const FGFGameData& GameData, const FString& Email, const FString& Password, FGFUserDataCallback TypedCallback);
UGameFuseUser::FetchUser(const int32 UserId, FGFUserDataCallback TypedCallback);
UGameFuseUser::AddScore(const int32 AddScore, FGFUserDataCallback TypedCallback);
UGameFuseUser::SetScore(const int32 SetScore, FGFUserDataCallback TypedCallback);
UGameFuseUser::AddCredits(const int32 AddCredits, FGFUserDataCallback TypedCallback);
UGameFuseUser::SetCredits(const int32 SetCredits, FGFUserDataCallback TypedCallback);
```

#### Store Items

```cpp
// Operations
UGameFuseUser::PurchaseStoreItem(const int32 StoreItemId, FGFStoreItemsCallback TypedCallback);
UGameFuseUser::RemoveStoreItem(const int32 StoreItemId, FGFStoreItemsCallback TypedCallback);
UGameFuseUser::FetchMyPurchasedStoreItems(FGFStoreItemsCallback TypedCallback);
UGameFuseUser::FetchUserPurchasedStoreItems(const int32 UserId, FGFStoreItemsCallback TypedCallback);
```

#### Attributes

```cpp
// Getters
UGameFuseUser::GetAttributeValue(const FString Key);

// Operations
UGameFuseUser::FetchMyAttributes(FGFAttributesCallback TypedCallback);
UGameFuseUser::FetchUserAttributes(const int32 UserId, FGFAttributesCallback TypedCallback);
UGameFuseUser::SyncLocalAttributes(FGFAttributesCallback TypedCallback);
UGameFuseUser::SetAttribute(const FString& Key, const FString& Value, FGFAttributesCallback TypedCallback);
UGameFuseUser::SetAttributes(const TMap<FString, FString>& NewAttributes, FGFAttributesCallback TypedCallback);
UGameFuseUser::SetAttributeLocal(const FString& SetKey, const FString& SetValue);
UGameFuseUser::RemoveAttribute(const FString& SetKey, FGFAttributesCallback TypedCallback);
UGameFuseUser::RemoveAttributes(const TArray<FString>& AttributeKeys, FGFAttributesCallback TypedCallback);
```

#### Leaderboards

```cpp
// Operations
UGameFuseUser::AddLeaderboardEntry(const FString& LeaderboardName, const int32 Score, const TMap<FString, FString>& Metadata, FGFInternalSuccessCallback TypedCallback);
UGameFuseUser::AddLeaderboardEntry(const FString& LeaderboardName, const int32 Score, FGFInternalSuccessCallback TypedCallback);
UGameFuseUser::ClearLeaderboardEntry(const FString& LeaderboardName, FGFInternalSuccessCallback TypedCallback);
UGameFuseUser::FetchMyLeaderboardEntries(const int32 Limit, bool bOnePerUser, FGFLeaderboardEntriesCallback TypedCallback);
UGameFuseUser::FetchUserLeaderboardEntries(const int32 UserId, const int32 Limit, bool bOnePerUser, FGFLeaderboardEntriesCallback TypedCallback);
```

### Blueprint Functions

#### Core User Data & Authentication

```cpp
UGameFuseUser::BP_SignUp(const FString& Email, const FString& Password, const FString& PasswordConfirmation, const FString& Username, FBP_GFApiCallback Callback);
UGameFuseUser::BP_SignIn(const FString& Email, const FString& Password, FBP_GFApiCallback Callback);
UGameFuseUser::BP_AddScore(const int32 Score, FBP_GFApiCallback Callback);
UGameFuseUser::BP_SetScore(const int32 Score, FBP_GFApiCallback Callback);
UGameFuseUser::BP_AddCredits(const int32 Credits, FBP_GFApiCallback Callback);
UGameFuseUser::BP_SetCredits(const int32 Credits, FBP_GFApiCallback Callback);
```

#### Store Items

```cpp
UGameFuseUser::BP_PurchaseStoreItem(const int32 StoreItemId, FBP_GFApiCallback Callback);
UGameFuseUser::BP_RemoveStoreItem(const int32 StoreItemId, FBP_GFApiCallback Callback);
UGameFuseUser::BP_FetchMyPurchasedStoreItems(FBP_GFApiCallback Callback);
UGameFuseUser::BP_FetchUserPurchasedStoreItems(const int32 UserId, FBP_GFApiCallback Callback);
```

#### Attributes

```cpp
UGameFuseUser::BP_SetAttribute(const FString& Key, const FString& Value, FBP_GFApiCallback Callback);
UGameFuseUser::BP_SetAttributes(const TMap<FString, FString>& NewAttributes, FBP_GFApiCallback Callback);
UGameFuseUser::BP_SetAttributeLocal(const FString& Key, const FString& Value);
UGameFuseUser::BP_RemoveAttribute(const FString& Key, FBP_GFApiCallback Callback);
UGameFuseUser::BP_RemoveAttributes(const TArray<FString>& AttributeKeys, FBP_GFApiCallback Callback);
UGameFuseUser::BP_FetchMyAttributes(FBP_GFApiCallback Callback);
UGameFuseUser::BP_FetchUserAttributes(const int32 UserId, FBP_GFApiCallback Callback);
UGameFuseUser::BP_SyncLocalAttributes(FBP_GFApiCallback Callback);
```

#### Leaderboards

```cpp
UGameFuseUser::BP_AddLeaderboardEntry(const FString& LeaderboardName, const int32 Score, FBP_GFApiCallback Callback);
UGameFuseUser::BP_AddLeaderboardEntryWithAttributes(const FString& LeaderboardName, const int32 Score, const TMap<FString, FString>& Metadata, FBP_GFApiCallback Callback);
UGameFuseUser::BP_ClearLeaderboardEntry(const FString& LeaderboardName, FBP_GFApiCallback Callback);
UGameFuseUser::BP_FetchMyLeaderboardEntries(const int32 Limit, bool bOnePerUser, FBP_GFApiCallback Callback);
UGameFuseUser::BP_FetchUserLeaderboardEntries(const int32 UserId, const int32 Limit, bool bOnePerUser, FBP_GFApiCallback Callback);
```

## GameFuseChat

### C++ Functions

#### Getters
```cpp
// Functions that just return internal data
UGameFuseChat::GetAllChats();
UGameFuseChat::GetChatMessages();
UGameFuseChat::GetRequestHandler();
```

#### Operations
```cpp
// Functions that perform API operations
UGameFuseChat::CreateChat(const TArray<FString>& ParticipantIds, const FString& InitialMessage, FGFChatCallback TypedCallback);
UGameFuseChat::SendMessage(int32 ChatId, const FString& Message, FGFMessageCallback TypedCallback);
UGameFuseChat::MarkMessageAsRead(int32 MessageId, FGFSuccessCallback TypedCallback);
UGameFuseChat::FetchAllChats(int32 Page, FGFChatListCallback TypedCallback);
UGameFuseChat::FetchMessages(int32 ChatId, int32 Page, FGFMessageListCallback TypedCallback);
UGameFuseChat::ClearChatData();
```

### Blueprint Functions

#### Operations
```cpp
// Blueprint-specific functions
UGameFuseChat::BP_CreateChat(const TArray<FString>& ParticipantIds, const FString& InitialMessage, const FBP_GFApiCallback& Callback);
UGameFuseChat::BP_SendMessage(int32 ChatId, const FString& Message, const FBP_GFApiCallback& Callback);
UGameFuseChat::BP_MarkMessageAsRead(int32 MessageId, const FBP_GFApiCallback& Callback);
UGameFuseChat::BP_FetchAllChats(int32 Page, const FBP_GFApiCallback& Callback);
UGameFuseChat::BP_FetchMessages(int32 ChatId, int32 Page, const FBP_GFApiCallback& Callback);
```

## GameFuseGroups

### C++ Functions

#### Getters
```cpp
// Functions that just return internal data
UGameFuseGroups::GetGroupById(const int32 GroupId, FGFGroup& OutGroup);
UGameFuseGroups::GetRequestHandler();
```

#### Operations
```cpp
// Functions that perform API operations
UGameFuseGroups::CreateGroup(const FGFGroup& Group, FGFGroupCallback TypedCallback);
UGameFuseGroups::FetchGroup(const int32 GroupId, FGFGroupCallback TypedCallback);
UGameFuseGroups::FetchAllGroups(FGFGroupListCallback TypedCallback);
UGameFuseGroups::RequestToJoinGroup(int32 GroupId, FGFGroupConnectionCallback TypedCallback);
UGameFuseGroups::DeleteGroup(const int32 GroupId, FGFGroupActionCallback TypedCallback);
UGameFuseGroups::JoinGroup(const int32 GroupId, FGFGroupActionCallback TypedCallback);
UGameFuseGroups::LeaveGroup(const int32 GroupId, FGFGroupActionCallback TypedCallback);
UGameFuseGroups::FetchMyGroups(FGFGroupListCallback TypedCallback);
UGameFuseGroups::SearchGroups(const FString& Query, FGFGroupListCallback TypedCallback);
UGameFuseGroups::AddAdmin(const int32 GroupId, const int32 UserId, FGFGroupActionCallback TypedCallback);
UGameFuseGroups::RemoveAdmin(const int32 GroupId, const int32 UserId, FGFGroupActionCallback TypedCallback);
UGameFuseGroups::AddAttribute(const int32 GroupId, const FGFGroupAttribute& Attribute, bool bOthersCanEdit, FGFGroupAttributeCallback TypedCallback);
UGameFuseGroups::UpdateGroupAttribute(int32 GroupId, const FGFGroupAttribute& Attribute, FGFGroupActionCallback TypedCallback);
UGameFuseGroups::FetchGroupAttributes(const int32 GroupId, FGFGroupAttributeCallback TypedCallback);
UGameFuseGroups::RespondToGroupJoinRequest(const int32 ConnectionId, const int32 UserId, EGFInviteRequestStatus Status, FGFGroupActionCallback TypedCallback);
```

### Blueprint Functions

#### Operations
```cpp
// Blueprint-specific functions
UGameFuseGroups::BP_CreateGroup(const FGFGroup& Group, const FBP_GFApiCallback& Callback);
UGameFuseGroups::BP_FetchGroup(const int32 GroupId, const FBP_GFApiCallback& Callback);
UGameFuseGroups::BP_FetchAllGroups(const FBP_GFApiCallback& Callback);
UGameFuseGroups::BP_RequestToJoinGroup(int32 GroupId, const FBP_GFApiCallback& Callback);
UGameFuseGroups::BP_DeleteGroup(const int32 GroupId, const FBP_GFApiCallback& Callback);
UGameFuseGroups::BP_JoinGroup(const int32 GroupId, const FBP_GFApiCallback& Callback);
UGameFuseGroups::BP_LeaveGroup(const int32 GroupId, const FBP_GFApiCallback& Callback);
UGameFuseGroups::BP_FetchUserGroups(const FBP_GFApiCallback& Callback);
UGameFuseGroups::BP_SearchGroups(const FString& Query, const FBP_GFApiCallback& Callback);
UGameFuseGroups::BP_AddAdmin(const int32 GroupId, const int32 UserId, const FBP_GFApiCallback& Callback);
UGameFuseGroups::BP_RemoveAdmin(const int32 GroupId, const int32 UserId, const FBP_GFApiCallback& Callback);
UGameFuseGroups::BP_AddAttribute(const int32 GroupId, const FGFGroupAttribute& Attribute, bool bOthersCanEdit, const FBP_GFApiCallback& Callback);
UGameFuseGroups::BP_UpdateGroupAttribute(const int32 GroupId, const FGFGroupAttribute& Attribute, const FBP_GFApiCallback& Callback);
UGameFuseGroups::BP_FetchGroupAttributes(const int32 GroupId, const FBP_GFApiCallback& Callback);
UGameFuseGroups::BP_AcceptGroupJoinRequest(const int32 ConnectionId, const int32 UserId, const FBP_GFApiCallback& Callback);
UGameFuseGroups::BP_DeclineGroupJoinRequest(const int32 ConnectionId, const int32 UserId, const FBP_GFApiCallback& Callback);
```

## GameFuseFriends

### C++ Functions

#### Getters
```cpp
// Functions that just return internal data
UGameFuseFriends::GetFriendsList();
UGameFuseFriends::GetOutgoingRequests();
UGameFuseFriends::GetIncomingRequests();
UGameFuseFriends::GetRequestHandler();
```

#### Operations
```cpp
// Functions that perform API operations
UGameFuseFriends::SendFriendRequest(const FString& Username, FGFFriendRequestCallback TypedCallback);
UGameFuseFriends::AcceptFriendRequest(const int32 FriendshipId, FGFFriendActionCallback TypedCallback);
UGameFuseFriends::DeclineFriendRequest(const int32 FriendshipId, FGFFriendActionCallback TypedCallback);
UGameFuseFriends::CancelFriendRequest(const int32 FriendshipId, FGFFriendActionCallback TypedCallback);
UGameFuseFriends::UnfriendPlayer(const int32 UserId, FGFFriendActionCallback TypedCallback);
UGameFuseFriends::FetchFriendshipData(FGFFriendsCallback TypedCallback);
UGameFuseFriends::FetchMyFriendsList(FGFFriendsCallback TypedCallback);
UGameFuseFriends::FetchUserFriendsList(int32 UserId, FGFFriendsCallback TypedCallback);
UGameFuseFriends::FetchOutgoingFriendRequests(FGFFriendRequestsCallback TypedCallback);
UGameFuseFriends::FetchIncomingFriendRequests(FGFFriendRequestsCallback TypedCallback);
```

### Blueprint Functions

#### Operations
```cpp
// Blueprint-specific functions
UGameFuseFriends::BP_SendFriendRequest(const FString& Username, const FBP_GFApiCallback& Callback);
UGameFuseFriends::BP_AcceptFriendRequest(int32 FriendshipId, const FBP_GFApiCallback& Callback);
UGameFuseFriends::BP_DeclineFriendRequest(int32 FriendshipId, const FBP_GFApiCallback& Callback);
UGameFuseFriends::BP_CancelFriendRequest(int32 FriendshipId, const FBP_GFApiCallback& Callback);
UGameFuseFriends::BP_UnfriendPlayer(int32 UserId, const FBP_GFApiCallback& Callback);
UGameFuseFriends::BP_FetchFriendshipData(const FBP_GFApiCallback& Callback);
UGameFuseFriends::BP_FetchMyFriendsList(const FBP_GFApiCallback& Callback);
UGameFuseFriends::BP_FetchUserFriendsList(int32 UserId, const FBP_GFApiCallback& Callback);
UGameFuseFriends::BP_FetchOutgoingFriendRequests(const FBP_GFApiCallback& Callback);
UGameFuseFriends::BP_FetchIncomingFriendRequests(const FBP_GFApiCallback& Callback);
```

## GameFuseRounds

### C++ Functions

#### Getters
```cpp
// Functions that just return internal data
UGameFuseRounds::GetUserRounds();
UGameFuseRounds::GetRequestHandler();
```

#### Operations
```cpp
// Functions that perform API operations
UGameFuseRounds::ClearRoundsData();
UGameFuseRounds::CreateGameRound(const FGFGameRound& GameRound, FGFGameRoundCallback TypedCallback);
UGameFuseRounds::CreateGameRound(const FGFGameRound& GameRound, const FGFUserData& UserData, FGFGameRoundCallback TypedCallback);
UGameFuseRounds::FetchGameRound(const int32 RoundId, FGFGameRoundCallback TypedCallback);
UGameFuseRounds::UpdateGameRound(const int32 RoundId, const FGFGameRound& GameRound, FGFGameRoundCallback TypedCallback);
UGameFuseRounds::FetchMyGameRounds(FGFGameRoundListCallback TypedCallback, const FString& GameType = "", int32 Page = 0, int32 PerPage = 0);
UGameFuseRounds::FetchUserGameRounds(int32 UserId, FGFGameRoundListCallback TypedCallback, const FString& GameType = "", int32 Page = 0, int32 PerPage = 0);
UGameFuseRounds::DeleteGameRound(const int32 RoundId, FGFGameRoundActionCallback TypedCallback);
```

### Blueprint Functions

#### Operations
```cpp
// Blueprint-specific functions
UGameFuseRounds::BP_CreateGameRound(const FGFGameRound& GameRound, const FBP_GFApiCallback& Callback);
UGameFuseRounds::BP_FetchGameRound(const int32 RoundId, const FBP_GFApiCallback& Callback);
UGameFuseRounds::BP_UpdateGameRound(const int32 RoundId, const FGFGameRound& GameRound, const FBP_GFApiCallback& Callback);
UGameFuseRounds::BP_FetchMyGameRounds(const FString& GameType, int32 Page, int32 PerPage, const FBP_GFApiCallback& Callback);
UGameFuseRounds::BP_FetchUserGameRounds(int32 UserId, const FString& GameType, int32 Page, int32 PerPage, const FBP_GFApiCallback& Callback);
UGameFuseRounds::BP_DeleteGameRound(const int32 RoundId, const FBP_GFApiCallback& Callback);
```

