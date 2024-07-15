# Class Methods

Check each model below for a list of methods and attributes.

```jsx
###GameFuseUser.js
your current signed in user can be retrieved with:
gameFuseUser user = GameFuse.CurrentUser;

isSignedIn();
getNumberOfLogins();
getLastLogin();
getUsername();
getScore();
getCredits();

addCredits(int credits, Action < string, bool > callback = null);
setCredits(int credits, Action < string, bool > callback = null);
addScore(int credits, Action < string, bool > callback = null);
setScore(int score, Action < string, bool > callback = null);
getAttributes();
setAttribute(string key, string value, Action < string, bool > callback = null);
setAttributeLocal(string key, string  val);
syncLocalAttributes(Action callback = null);
setAttributes(Dictionary newAttributes, Action callback = null);
getDirtyAttributes();
getAttributesKeys();
getAttributeValue(string key);
setAttribute(string key, string value, Action < string, bool > callback = null);
removeAttribute(string key, Action < string, bool > callback = null);
getPurchasedStoreItems();
purchaseStoreItem(GameFuseStoreItem storeItem, Action < string, bool > callback = null);
purchaseStoreItem(int storeItemId, Action < string, bool > callback = null);
removeStoreItem(int storeItemID, bool reimburseUser, Action < string, bool > callback = null);
removeStoreItem(GameFuseStoreItem storeItem, bool reimburseUser, Action < string, bool > callback = null);
addLeaderboardEntry(string leaderboardName, int score, Dictionary extraAttributes = null, Action < string, bool > callback = null);
addLeaderboardEntry(string leaderboardName, int score, Action < string, bool > callback = null);
getLeaderboard(int limit, bool onePerUser, Action < string, bool > callback = null); //Get all leaderboard entries for current signed in user

###GameFuse.js
setUpGame(string gameId, string token, function(string, bool) callback = null);
getGameId();
getGameName();
getGameDescription();
getStoreItems() //Gets all store items (your library)
signIn(string email, string password, function(string, bool) callback = null);
signUp(string email, string password, string password_confirmation, string username, function(string, bool) callback = null);
getLeaderboard(int limit, bool onePerUser, string LeaderboardName, function(string, bool) callback = null); //Retrieves leaderboard for one specific Leaderboard Name
sendPasswordResetEmail(string email, function(string, bool) callback = null)
fetchGameVariables(gameId, token, callback = undefined, extraData={})
getGameVariables()

###GameFuseStoreItem.js
getName();
getCategory();
getDescription();
getCost();
getId();
getIconUrl();

###GameFuseLeaderboardEntry.js
getUsername();
getScore();
getLeaderboardName();
getExtraAttributes();
getTimestamp();
```