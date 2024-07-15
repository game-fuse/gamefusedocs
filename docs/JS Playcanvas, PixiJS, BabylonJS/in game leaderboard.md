# In game leaderboard

Leaderboards can be easily created within GameFuse From any JS game client, a Leaderboard Entry can be added with a leaderboard_name, score, and extra_attributes (metadata) for the current signed in user Leaderboards can be downloaded for a specific leaderboard_name, which would gather all the high scores in order for all users in the game or Leaderboards can be downloaded for a specific user, so that you can download the current users leaderboard data for all leaderboard_names The below example shows submitting 2 leaderboard entries, then retrieving them for the game, and for the current user

```jsx
Start(){
  let self = this
  var extraAttributes = {};
  extraAttributes["deaths"] =  "15";
  extraAttributes["Jewels"] =  "12";
  GameFuseUser.CurrentUser.AddLeaderboardEntry("Game1Leaderboard",10, extraAttributes, function(message,hasError){self.LeaderboardEntryAdded(message,hasError)});
}

LeaderboardEntryAdded(message, hasError)
{
    let self = this;
    if (hasError)
    {
        print("Error adding leaderboard entry: " + message);
    }
    else
    {

        print("Set Leaderboard Entry 2");
        var extraAttributes = {};
        extraAttributes.["deaths"] = "25";
        extraAttributes.["Jewels"] = "15";

        GameFuseUser.CurrentUser.AddLeaderboardEntry("Game1Leaderboard", 7, extraAttributes, function(message,hasError){self.LeaderboardEntryAdded2(message,hasError)});

    }
}

LeaderboardEntryAdded2(message, hasError)
{
    let self = this;
    if (hasError)
    {
        print("Error adding leaderboard entry 2: " + message);
    }
    else
    {
        print("Set Leaderboard Entry 2");
        GameFuseUser.CurrentUser.GetLeaderboard(5, true, function(message,hasError){self.LeaderboardEntriesRetrieved(message,hasError)});
    }
}

LeaderboardEntriesRetrieved(message, hasError)
{
    if (hasError)
    {
        print("Error loading leaderboard entries: " + message);
    }
    else
    {
        let self = this;
        print("Got leaderboard entries for specific user!");
        for (const entry of GameFuse.Instance.leaderboardEntries) {
            console.log(entry.getUsername() + ": " + entry.getScore().toString() + ": " + entry.getLeaderboardName());
            const extraAttributes = entry.getExtraAttributes();
            for (const key in extraAttributes) {
                console.log(key + ": " + extraAttributes[key]);
            }

        }
        GameFuse.Instance.GetLeaderboard(5, true, "Game1Leaderboard", function(message,hasError){self.LeaderboardEntriesRetrievedAll(message,hasError)});

    }
}

LeaderboardEntriesRetrievedAll(message, hasError)
{
    if (hasError)
    {
        print("Error loading leaderboard entries: " + message);
    }
    else
    {
        let self = this;
        print("Got leaderboard entries for whole game!");
        for (const entry of GameFuse.Instance.leaderboardEntries) {
            console.log(entry.getUsername() + ": " + entry.getScore().toString() + ": " + entry.getLeaderboardName());
            const extraAttributes = entry.getExtraAttributes();
            for (const key in extraAttributes) {
                console.log(key + ": " + extraAttributes[key]);
            }

        }

    }
}

```

You can also clear all leaderboard entries for a particular leaderboard_name for the current user like this:

```jsx
Start(){
  let self = this;
  var extraAttributes = {};
  extraAttributes["deaths"] = "15";
  extraAttributes["Jewels"] = "12";
  GameFuseUser.CurrentUser.AddLeaderboardEntry("Game2Leaderboard",10, extraAttributes, function(message,hasError){self.LeaderboardEntryAdded(message,hasError)});
}

LeaderboardEntryAdded(message, hasError)
{
    let self = this;
    if (hasError)
    {
        print("Error adding leaderboard entry: " + message);
    }
    else
    {
        print("Clear Leaderboard Entry 2");
        GameFuseUser.CurrentUser.ClearLeaderboardEntries("Game2Leaderboard", function(message,hasError){self.LeaderboardEntryCleared(message,hasError)});
    }
}

LeaderboardEntryCleared(message, hasError)
{
    if (hasError)
    {
        print("Error adding leaderboard entry: " + message);
    }
    else
    {
        print("User will no longer have leaderboard entries for 'Game2Leaderboard'");

    }
}

```

```
# GameFuseUser.CurrentUser.AddLeaderboardEntry
* 401 - "can only add entries for current user"
* 400 - "invalid extra attributes"
* 500 - unknown server error
# GameFuseUser.CurrentUser.GetLeaderboard
* 401 - "can only get entries for current user"
* 500 - unknown server error
# GameFuse.Instance.GetLeaderboard
* 404 - No entries for this leaderboard name ___
* 500 - unknown server error
# GameFuseUser.CurrentUser.ClearLeaderboardEntries
* 401 - "can only clear entries for current user"
* 500 - unknown server error
```