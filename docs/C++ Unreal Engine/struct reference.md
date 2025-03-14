# Struct Reference

This page provides a reference for all the public structs returned by GameFuse functions, along with their fields and types.

## FGFGameData

Game information returned by GameFuseManager.

```cpp
struct FGFGameData
{
    int32 Id;                  // The unique identifier of the game
    FString Token;             // The API token for the game
    FString Name;              // The name of the game
    FString Description;       // The description of the game
}
```

## FGFUserData

User information returned by GameFuseUser.

```cpp
struct FGFUserData
{
    int32 Id;                  // The unique identifier of the user
    FString Username;          // The username of the user
    bool bSignedIn;            // Whether the user is signed in
    int32 NumberOfLogins;      // The number of times the user has logged in
    FString AuthenticationToken; // The authentication token for the user
    int32 Score;               // The user's score
    int32 Credits;             // The user's credits
    FString LastLogin;         // The date of the user's last login
}
```

## FGFStoreItem

Store item information returned by GameFuseManager and GameFuseUser.

```cpp
struct FGFStoreItem
{
    int32 Id;                  // The unique identifier of the store item
    FString Name;              // The name of the store item
    FString Category;          // The category of the store item
    FString Description;       // The description of the store item
    int32 Cost;                // The cost of the store item in credits
    FString IconUrl;           // The URL of the store item's icon
}
```

## FGFLeaderboardEntry

Leaderboard entry information returned by GameFuseManager and GameFuseUser.

```cpp
struct FGFLeaderboardEntry
{
    FString LeaderboardName;   // The name of the leaderboard
    FString Username;          // The username of the user who made the entry
    int32 Score;               // The score of the entry
    int32 GameUserId;          // The ID of the user who made the entry
    TMap<FString, FString> Metadata; // Additional metadata for the entry
    FString DateTime;          // The date and time when the entry was made
}
```

## FGFLeaderboard

Leaderboard information returned by GameFuseManager.

```cpp
struct FGFLeaderboard
{
    FString Name;              // The name of the leaderboard
    TArray<FGFLeaderboardEntry> Entries; // The entries in the leaderboard
}
```

## FGFAttributeList

Attribute list information returned by GameFuseUser.

```cpp
struct FGFAttributeList
{
    TMap<FString, FString> Attributes; // Map of attribute keys to values
}
```

## FGFMessage

Message information returned by GameFuseChat.

```cpp
struct FGFMessage
{
    int32 Id;                  // The unique identifier of the message
    FString Text;              // The text content of the message
    int32 UserId;              // The ID of the user who sent the message
    FDateTime CreatedAt;       // When the message was created
    TArray<int32> ReadBy;      // List of user IDs who have read the message
    bool bRead;                // Whether the current user has read the message
}
```

## FGFChat

Chat information returned by GameFuseChat.

```cpp
struct FGFChat
{
    int32 Id;                  // The unique identifier of the chat
    int32 CreatorId;           // The ID of the user who created the chat
    FString CreatorType;       // The type of creator (User, etc.)
    TArray<FGFMessage> Messages; // List of messages in the chat
    TArray<FGFUserData> Participants; // List of users in the chat
}
```

## FGFGroup

Group information returned by GameFuseGroups.

```cpp
struct FGFGroup
{
    int32 Id;                  // The unique identifier of the group
    FString Name;              // The name of the group
    FString GroupType;         // The type of the group
    int32 MaxGroupSize;        // The maximum number of members allowed in the group
    bool bCanAutoJoin;         // Whether users can automatically join the group
    bool bIsInviteOnly;        // Whether the group is invite-only
    bool bSearchable;          // Whether the group is searchable
    bool bAdminsOnlyCanCreateAttributes; // Whether only admins can create attributes
    int32 MemberCount;         // The number of members in the group
    TArray<FGFUserData> Members; // List of members in the group
    TArray<FGFUserData> Admins; // List of admins in the group
    TArray<FGFGroupAttribute> Attributes; // List of attributes for the group
}
```

## FGFGroupAttribute

Group attribute information used by GameFuseGroups.

```cpp
struct FGFGroupAttribute
{
    int32 Id;                  // The unique identifier of the attribute
    FString Key;               // The key of the attribute
    FString Value;             // The value of the attribute
    bool bOnlyCreatorCanEdit;  // Whether only the creator can edit the attribute
}
```

## FGFGroupConnection

Group connection information returned by GameFuseGroups.

```cpp
struct FGFGroupConnection
{
    int32 Id;                  // The unique identifier of the connection
    EGFInviteRequestStatus Status; // The status of the connection
    FGFUserData User;          // The user associated with the connection
}
```

## FGFFriendRequest

Friend request information returned by GameFuseFriends.

```cpp
struct FGFFriendRequest
{
    int32 Id;                  // The unique identifier of the friend request
    int32 SenderId;            // The ID of the user who sent the request
    int32 ReceiverId;          // The ID of the user who received the request
    FString Status;            // The status of the request
    FGFUserData User;          // The user associated with the request
}
```

## FGFGameRound

Game round information returned by GameFuseRounds.

```cpp
struct FGFGameRound
{
    int32 Id;                  // The unique identifier of the game round
    int32 GameUserId;          // The ID of the user who played the round
    FDateTime StartTime;       // When the round started
    FDateTime EndTime;         // When the round ended
    int32 Score;               // The score achieved in the round
    int32 Place;               // The place achieved in the round
    FString GameType;          // The type of game played
    int32 MultiplayerGameRoundId; // The ID of the multiplayer game round
    TMap<FString, FString> Metadata; // Additional metadata for the round
    bool bMultiplayer;         // Whether the round was multiplayer
    TArray<FGFGameRoundRanking> Rankings; // List of rankings for the round
}
```

## FGFGameRoundRanking

Game round ranking information used by GameFuseRounds.

```cpp
struct FGFGameRoundRanking
{
    int32 Place;               // The place achieved in the ranking
    int32 Score;               // The score achieved in the ranking
    FDateTime StartTime;       // When the player started the round
    FDateTime EndTime;         // When the player ended the round
    FGFUserData User;          // The user associated with the ranking
}
```

## FGFAPIResponse

API response information returned by all GameFuse API calls.

```cpp
struct FGFAPIResponse
{
    bool bSuccess;             // Whether the API call was successful
    FString ResponseStr;       // The response string from the API
    FGuid RequestId;           // The unique identifier of the request
    int ResponseCode;          // The HTTP response code
}
``` 