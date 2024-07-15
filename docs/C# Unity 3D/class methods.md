# Class Methods

Check each model below for a list of methods and attributes.

```csharp
###GameFuse.cs
your current signed in user can be retrieved with:
GameFuseUser user = GameFuse.CurrentUser;

public bool IsSignedIn();
public int GetNumberOfLogins();
public DateTime GetLastLogin();
public string GetUsername();
public int GetScore();
public int GetCredits();

public void AddCredits(int credits, Action < string, bool > callback = null);
public void SetCredits(int credits, Action < string, bool > callback = null);
public void AddScore(int credits, Action < string, bool > callback = null);
public void SetScore(int score, Action < string, bool > callback = null);
public Dictionary < string,string >  GetAttributes();
public Dictionary < string,string > .KeyCollection GetAttributesKeys();
public string GetAttributeValue(string key);
public void SetAttribute(string key, string value, Action < string, bool > callback = null);
public void SetAttributeLocal(string key, string  val);
public void SyncLocalAttributes(Action callback = null);
public void SetAttributes(Dictionary newAttributes, Action callback = null);
public Dictionary GetDirtyAttributes();
public void RemoveAttribute(string key, Action < string, bool > callback = null);
public List < GameFuseStoreItem > GetPurchasedStoreItems();
public void PurchaseStoreItem(GameFuseStoreItem storeItem, Action < string, bool > callback = null);
public void PurchaseStoreItem(int storeItemId, Action < string, bool > callback = null);
public void RemoveStoreItem(int storeItemID, bool reimburseUser, Action < string, bool > callback = null);
public void RemoveStoreItem(GameFuseStoreItem storeItem, bool reimburseUser, Action < string, bool > callback = null);
public void AddLeaderboardEntry(string leaderboardName, int score, Dictionary extraAttributes = null, Action < string, bool > callback = null);
public void AddLeaderboardEntry(string leaderboardName, int score, Action < string, bool > callback = null);
public void GetLeaderboard(int limit, bool onePerUser, Action < string, bool > callback = null); //Get all leaderboard entries for current signed in user

###GameFuse.cs
public static void SetUpGame(string gameId, string token, Action < string, bool > callback = null);
public static string GetGameId();
public static string GetGameName();
public static string GetGameDescription();
public static List < GameFuseStoreItem > GetStoreItems() //Gets all store items (your library)
public static void SignIn(string email, string password, Action < string, bool > callback = null);
public static void SignUp(string email, string password, string password_confirmation, string username, Action < string, bool > callback = null);
public void GetLeaderboard(int limit, bool onePerUser, string LeaderboardName, Action < string, bool > callback = null); //Retrieves leaderboard for one specific Leaderboard Name
public static void SendPasswordResetEmail(string email, Action < string, bool > callback = null);
public static void FetchGameVariables(string gameId, string token, Action < string, bool > callback = null)
public static string GetGameVariable(string key)

###GameFuseStoreItem.cs
public string GetName();
public string GetCategory();
public string GetDescription();
public int GetCost();
public int GetId();
public string GetIconUrl();

###GameFuseLeaderboardEntry.cs
public string GetUsername();
public int GetScore();
public string GetLeaderboardName();
public Dictionary GetExtraAttributes();
public DateTime GetTimestamp();
```