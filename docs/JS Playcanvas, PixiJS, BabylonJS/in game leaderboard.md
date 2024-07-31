Leaderboards can be easily created within GameFuse from the Unity game client.
A leaderboard entry can be added with:

- `leaderboard_name`
- `score`
- `extra_attributes` (metadata)

for the current signed in user.

Leaderboards can be downloaded for a specific `leaderboard_name`, which would
gather and sort the high scores for all users in the game. Leaderboards can
also be downloaded for a specific user.

The example below shows submitting 2 leaderboard entries, then retrieving them
for the game, and for the current user.

!!! example
    ```jsx
    Start()
    {
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
            for (const entry of GameFuse.Instance.leaderboardEntries)
            {
                console.log(entry.getUsername() + ": " + entry.getScore().toString() + ": " + entry.getLeaderboardName());
                const extraAttributes = entry.getExtraAttributes();
                for (const key in extraAttributes)
                {
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
            for (const entry of GameFuse.Instance.leaderboardEntries)
            {
                console.log(entry.getUsername() + ": " + entry.getScore().toString() + ": " + entry.getLeaderboardName());
                const extraAttributes = entry.getExtraAttributes();
                for (const key in extraAttributes) {
                    console.log(key + ": " + extraAttributes[key]);
                }
            }
        }
    }
    ```

You can also clear all leaderboard entries in a specific leaderboard for the
current user like this:

!!! example
    ```jsx
    Start()
    {
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

## Function return values

### `GameFuseUser.CurrentUser.AddLeaderboardEntry`

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK |
| `400`            | Invalid extra attributes |
| `401`            | Can only add entries for current user |
| `500`            | Unknown server error |

### `GameFuseUser.CurrentUser.GetLeaderboard`

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK |
| `401`            | Can only get entries for the current user |
| `500`            | Unknown server error |

### `GameFuseUser.CurrentUser.ClearLeaderboardEntries`

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK |
| `401`            | Can only clear entries for the current user |
| `500`            | Unknown server error |

### `GameFuse.Instance.GetLeaderboard`

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK |
| `404`            | No entries for this leaderboard name |
| `500`            | Unknown server error |
