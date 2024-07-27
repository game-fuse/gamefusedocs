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
    ```csharp
    void Start()
    {
        var extraAttributes = new Dictionary < string, string > ();
        extraAttributes.Add("deaths", "15");
        extraAttributes.Add("Jewels", "12");
        GameFuseUser.CurrentUser.AddLeaderboardEntry("Game1Leaderboard", 10, extraAttributes, LeaderboardEntryAdded);
    }

    void LeaderboardEntryAdded(string message, bool hasError)
    {
        if (hasError)
        {
            print("Error adding leaderboard entry: " + message);
        }
        else
        {

            print("Set Leaderboard Entry 2");
            var extraAttributes = new Dictionary < string, string > ();
            extraAttributes.Add("deaths", "25");
            extraAttributes.Add("Jewels", "15");

            GameFuseUser.CurrentUser.AddLeaderboardEntry("Game1Leaderboard", 7, extraAttributes, LeaderboardEntryAdded2);

        }
    }

    void LeaderboardEntryAdded2(string message, bool hasError)
    {
        if (hasError)
        {
            print("Error adding leaderboard entry 2: " + message);
        }
        else
        {
            print("Set Leaderboard Entry 2");
            GameFuseUser.CurrentUser.GetLeaderboard(5, true, LeaderboardEntriesRetrieved);
        }
    }

    void LeaderboardEntriesRetrieved(string message, bool hasError)
    {
        if (hasError)
        {
            print("Error loading leaderboard entries: " + message);
        }
        else
        {

            print("Got leaderboard entries for specific user!");
            foreach( GameFuseLeaderboardEntry entry in GameFuse.Instance.leaderboardEntries)
            {
                print(entry.GetUsername() + ": " + entry.GetScore().ToString() + ": " + entry.GetLeaderboardName() );
                foreach (KeyValuePair < string,string > kvPair in entry.GetExtraAttributes())
                {
                    print(kvPair.Key + ": " + kvPair.Value);
                }

            }
            GameFuse.Instance.GetLeaderboard(5, true, "Game1Leaderboard", LeaderboardEntriesRetrievedAll);

        }
    }

    void LeaderboardEntriesRetrievedAll(string message, bool hasError)
    {
        if (hasError)
        {
            print("Error loading leaderboard entries: " + message);
        }
        else
        {
            print("Got leaderboard entries for whole game!");
            foreach (GameFuseLeaderboardEntry entry in GameFuse.Instance.leaderboardEntries)
            {
                print(entry.GetUsername() + ": " + entry.GetScore().ToString() + ": " + entry.GetLeaderboardName());
                foreach (KeyValuePair < string, string > kvPair in entry.GetExtraAttributes())
                {
                    print(kvPair.Key + ": " + kvPair.Value);
                }

            }

        }
    }
    ```

You can also clear all leaderboard entries in a specific leaderboard for the
current user like this:

!!! example
    ```csharp
    void Start(){
      var extraAttributes = new Dictionary < string, string > ();
      extraAttributes.Add("deaths", "15");
      extraAttributes.Add("Jewels", "12");
      GameFuseUser.CurrentUser.AddLeaderboardEntry("Game2Leaderboard",10, extraAttributes, LeaderboardEntryAdded);
    }

    void LeaderboardEntryAdded(string message, bool hasError)
    {
        if (hasError)
        {
            print("Error adding leaderboard entry: " + message);
        }
        else
        {

            print("Clear Leaderboard Entry 2");
            GameFuseUser.CurrentUser.ClearLeaderboardEntries("Game2Leaderboard", LeaderboardEntryCleared);

        }
    }

    void LeaderboardEntryCleared(string message, bool hasError)
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
