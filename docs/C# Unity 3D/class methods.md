# Class Methods

This document provides a comprehensive list of all public methods available in the GameFuse SDK for Unity C#, organized by functionality.

## Authentication & Account Management

### Sign Up and Sign In

```csharp
// Sign up a new user
public static async Task<GameFuseUser> SignUpAsync(string email, string password, string username, string gameId = null, string gameApiKey = null, CancellationToken cancellationToken = default)

// Sign in an existing user  
public static async Task<GameFuseUser> SignInAsync(string emailOrUsername, string password, string gameId = null, string gameApiKey = null, CancellationToken cancellationToken = default)

// Sign out the current user (static)
public static void SignOut(CancellationToken cancellationToken = default)

// Sign out this user instance
public void SignOut()

// Check if user is authenticated
public bool IsAuthenticated()
```

### Password Management

```csharp
// Initiate forgot password process
public static Task ForgotPasswordAsync(string email, string gameId = null, string gameApiKey = null, CancellationToken cancellationToken = default)

// Reset password using token from email
public static Task ResetPasswordAsync(string token, string password, string gameId = null, string gameApiKey = null, CancellationToken cancellationToken = default)

// Update current user's password
public async Task<User> UpdatePasswordAsync(string currentPassword, string newPassword, CancellationToken cancellationToken = default)
```

## User Profile & Data Management

### Profile Information

```csharp
// Get current user details (refresh from server)
public async Task<User> GetUserDetailsAsync(CancellationToken cancellationToken = default)

// Get details for a specific user by ID
public async Task<User> GetUserDetailsAsync(int userIdToFetch, CancellationToken cancellationToken = default)

// Update current user's profile
public async Task<User> UpdateProfileAsync(string username = null, string email = null, CancellationToken cancellationToken = default)
```

### User Properties

```csharp
// Access user properties (read-only)
public int Id { get; }
public string Username { get; }
public string Email { get; }
public string DisplayEmail { get; }
public int Credits { get; }
public int Score { get; }
public string LastLogin { get; }
public int NumberOfLogins { get; }
public int EventsTotal { get; }
public int EventsCurrentMonth { get; }
public int GameSessionsTotal { get; }
public int GameSessionsCurrentMonth { get; }
```

### Custom Attributes

```csharp
// Set a single custom attribute
public async Task<User> SetUserAttributeAsync(string key, string value, CancellationToken cancellationToken = default)

// Set multiple attributes at once
public async Task<User> SetUserAttributesBatchAsync(List<KeyValuePair<string, string>> attributesToSet, CancellationToken cancellationToken = default)

// Get all custom attributes for current user
public async Task<UserAttributes> GetUserAttributesAsync(CancellationToken cancellationToken = default)

// Get custom attributes for a specific user
public async Task<UserAttributes> GetUserAttributesAsync(int userIdToFetch, CancellationToken cancellationToken = default)

// Delete a user attribute
public async Task<User> DeleteUserAttributeAsync(string attributeKey, CancellationToken cancellationToken = default)
```

### Score Management

```csharp
// Add to current score
public async Task<User> AddScoreAsync(int scoreAmount, CancellationToken cancellationToken = default)

// Set absolute score value
public async Task<User> SetScoreAsync(int scoreAmount, CancellationToken cancellationToken = default)
```

### Credits Management

```csharp
// Add to current credits
public async Task<User> AddCreditsAsync(int creditsAmount, CancellationToken cancellationToken = default)

// Set absolute credits value
public async Task<User> SetCreditsAsync(int newCredits, CancellationToken cancellationToken = default)
```

## Store & Purchases

### Store Items

```csharp
// Get all available store items (requires game credentials)
public async Task<IReadOnlyList<StoreItem>> GetAvailableStoreItemsAsync(string gameId, string gameToken, CancellationToken cancellationToken = default)

// Get all available store items (uses settings)
public async Task<IReadOnlyList<StoreItem>> GetAvailableStoreItemsAsync(CancellationToken cancellationToken = default)

// Get current user's purchased store items
public async Task<UserStore> GetUserStoreItemsAsync(CancellationToken cancellationToken = default)

// Get store items for a specific user
public async Task<UserStore> GetUserStoreItemsAsync(int userIdToFetch, CancellationToken cancellationToken = default)
```

### Purchase Management

```csharp
// Purchase a store item
public async Task<UserStore> PurchaseStoreItemAsync(int storeItemId, CancellationToken cancellationToken = default)

// Remove a purchased store item
public async Task<UserStore> RemoveStoreItemAsync(int storeItemId, CancellationToken cancellationToken = default)
```

## Friends & Social

### Friend Requests

```csharp
// Send a friend request
public async Task<FriendshipResponse> SendFriendRequestAsync(string friendUsername, CancellationToken cancellationToken = default)

// Accept a friend request
public async Task<FriendshipStatusResponse> AcceptFriendRequestAsync(int friendshipId, CancellationToken cancellationToken = default)

// Decline a friend request
public async Task<FriendshipStatusResponse> DeclineFriendRequestAsync(int friendshipId, CancellationToken cancellationToken = default)

// Cancel a sent friend request
public async Task<FriendshipStatusResponse> CancelFriendRequestAsync(int friendshipId, CancellationToken cancellationToken = default)
```

### Friends Management

```csharp
// Get complete friendship data (friends, incoming/outgoing requests)
public async Task<FriendshipDataResponse> GetFriendshipDataAsync(CancellationToken cancellationToken = default)

// Get friends list for current user
public async Task<IReadOnlyList<Friend>> GetFriendsListAsync(CancellationToken cancellationToken = default)

// Get friends list for another user
public async Task<IReadOnlyList<Friend>> GetFriendsListForOtherUserAsync(int otherUserId, CancellationToken cancellationToken = default)

// Get incoming friend requests
public async Task<IReadOnlyList<FriendRequest>> GetIncomingFriendRequestsAsync(CancellationToken cancellationToken = default)

// Get outgoing friend requests
public async Task<IReadOnlyList<FriendRequest>> GetOutgoingFriendRequestsAsync(CancellationToken cancellationToken = default)

// Remove a friend
public async Task<FriendshipResponse> UnfriendPlayerAsync(int friendUserId, CancellationToken cancellationToken = default)
```

## Groups

### Group Management

```csharp
// Create a new group
public async Task<Group> CreateGroupAsync(CreateGroupPayload payload, CancellationToken cancellationToken = default)

// Fetch all available groups
public async Task<IReadOnlyList<GroupSummary>> FetchAllGroupsAsync(CancellationToken cancellationToken = default)

// Get detailed group information
public async Task<Group> FetchGroupDetailsAsync(int groupId, CancellationToken cancellationToken = default)
```

### Group Membership

```csharp
// Send request to join a group
public async Task<GroupConnectionResponse> SendGroupConnectionRequestAsync(int groupId, CancellationToken cancellationToken = default)

// Accept a group membership request (admin action)
public async Task<GroupConnectionStatusUpdateResponse> AcceptGroupMembershipRequestAsync(int groupConnectionId, CancellationToken cancellationToken = default)

// Decline a group membership request (admin action)
public async Task<GroupConnectionStatusUpdateResponse> DeclineGroupMembershipRequestAsync(int groupConnectionId, CancellationToken cancellationToken = default)
```

### Group Attributes

```csharp
// Create multiple group attributes
public async Task<CreateGroupAttributesResponse> CreateGroupAttributesAsync(int groupId, List<GroupAttributePayloadItem> attributesToCreate, CancellationToken cancellationToken = default)

// Create a single group attribute
public async Task<GroupAttributeResponseItem> CreateGroupAttributeAsync(int groupId, string key, string value, bool? othersCanEdit = null, CancellationToken cancellationToken = default)

// Get all group attributes
public async Task<IReadOnlyList<GroupAttributeResponseItem>> FetchGroupAttributesAsync(int groupId, CancellationToken cancellationToken = default)

// Modify a group attribute
public async Task<GroupAttributeResponseItem> ModifyGroupAttributeAsync(int groupId, ModifyGroupAttributePayload payload, CancellationToken cancellationToken = default)
```

## Game Rounds

### Game Round Management

```csharp
// Create a non-multiplayer game round
public Task<GameRound> CreateGameRoundAsync(string gameType, string startTime = null, string endTime = null, int? score = null, int? place = null, Dictionary<string, object> metadata = null, CancellationToken cancellationToken = default)

// Create or join a multiplayer game round
public Task<GameRound> CreateMultiplayerGameRoundAsync(string gameType, string startTime = null, string endTime = null, int? score = null, int? place = null, Dictionary<string, object> metadata = null, int? multiplayerGameRoundId = null, CancellationToken cancellationToken = default)

// Get a specific game round
public Task<GameRound> GetGameRoundAsync(int gameRoundId, CancellationToken cancellationToken = default)

// Update a game round
public Task<GameRound> UpdateGameRoundAsync(int gameRoundId, string startTime = null, string endTime = null, int? score = null, int? place = null, string gameType = null, Dictionary<string, object> metadata = null, CancellationToken cancellationToken = default)

// Delete a game round
public Task<GameRoundDeleteResponse> DeleteGameRoundAsync(int gameRoundId, CancellationToken cancellationToken = default)
```

### Game Round Queries

```csharp
// Get game rounds for current user
public Task<IReadOnlyList<GameRound>> GetCurrentUserGameRoundsAsync(int page = 1, int perPage = 100, CancellationToken cancellationToken = default)

// Get game rounds for a specific user
public Task<IReadOnlyList<GameRound>> GetGameRoundsForUserAsync(int userId, int page = 1, int perPage = 100, CancellationToken cancellationToken = default)

// Get game leaderboard
public Task<IReadOnlyList<LeaderboardEntry>> GetLeaderboardAsync(int limit = 100, CancellationToken cancellationToken = default)

// Get current user's rank
public Task<LeaderboardEntry> GetUserRankAsync(CancellationToken cancellationToken = default)
```

## Leaderboards

### Leaderboard Management

```csharp
// Submit a leaderboard entry
public Task<User> SubmitLeaderboardEntryAsync(string leaderboardName, double score, Dictionary<string, object> metadata = null, CancellationToken cancellationToken = default)

// Clear all leaderboard entries for current user
public Task<User> ClearLeaderboardEntriesAsync(string leaderboardName, CancellationToken cancellationToken = default)

// Get leaderboard entries for a specific leaderboard
public Task<LeaderboardEntriesResponse> GetLeaderboardEntriesAsync(int gameId, string leaderboardName, int limit, CancellationToken cancellationToken = default)
```

### Leaderboard Queries

```csharp
// Get current user's leaderboard entries
public Task<LeaderboardEntriesResponse> GetCurrentUserLeaderboardEntriesAsync(int limit, string leaderboardName = null, bool? onePerUser = null, CancellationToken cancellationToken = default)

// Get leaderboard entries for a specific user
public Task<LeaderboardEntriesResponse> GetUserLeaderboardEntriesAsync(int userId, int limit, string leaderboardName = null, bool? onePerUser = null, CancellationToken cancellationToken = default)

// Get user leaderboard entries (alternative method)
public async Task<LeaderboardEntries> GetUserLeaderboardEntriesAsync(string leaderboardName = null, int? limit = null, bool? onePerUser = null, CancellationToken cancellationToken = default)

// Get leaderboard entries for specific user by ID
public async Task<LeaderboardEntries> GetUserLeaderboardEntriesAsync(int userIdToFetch, string leaderboardName = null, int? limit = null, bool? onePerUser = null, CancellationToken cancellationToken = default)
```

## Messaging & Chat

### Chat Management

```csharp
// Get paginated list of chats
public Task<PaginatedChatsResponse> FetchMyPaginatedChatsAsync(int page = 1, CancellationToken cancellationToken = default)

// Create a direct chat with users
public async Task<Chat> CreateDirectChatAsync(List<string> usernames, string initialMessageText, CancellationToken cancellationToken = default)

// Create a group chat
public async Task<Chat> CreateGroupChatAsync(int groupId, string initialMessageText, CancellationToken cancellationToken = default)
```

### Message Management

```csharp
// Get paginated messages for a chat
public Task<PaginatedMessagesResponse> FetchPaginatedMessagesForChatAsync(int chatId, int page = 1, CancellationToken cancellationToken = default)

// Send a message to a chat
public Task<Message> SendMessageToChatAsync(int chatId, string messageText, CancellationToken cancellationToken = default)

// Mark a message as read
public Task<MarkAsReadResponse> MarkMessageAsReadAsync(int messageId, CancellationToken cancellationToken = default)
```

## Static Access

### Current User Access

```csharp
// Get the currently authenticated user
public static GameFuseUser CurrentUser { get; private set; }
```

## Collection Properties

### User Collections

```csharp
// Read-only collections accessible via properties
public IReadOnlyList<UserAttribute> GameUserAttributes { get; }
public IReadOnlyList<StoreItem> GameUserStoreItems { get; }
public IReadOnlyList<Friend> Friends { get; }
public IReadOnlyList<FriendRequest> OutgoingFriendRequests { get; }
public IReadOnlyList<FriendRequest> IncomingFriendRequests { get; }
public IReadOnlyList<GroupSummary> Groups { get; }
public IReadOnlyList<GroupConnectionResponse> GroupJoinRequests { get; }
public IReadOnlyList<MyGroupInvite> GroupInvites { get; }
```