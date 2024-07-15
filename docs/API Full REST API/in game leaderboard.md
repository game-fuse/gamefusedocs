TLeaderboards can be easily created within GameFuse From your JS game client, a Leaderboard Entry can be added with a leaderboard_name, score, and extra_attributes (metadata) for the current signed in user Leaderboards can be downloaded for a specific leaderboard_name, which would gather all the high scores in order for all users in the game or Leaderboards can be downloaded for a specific user, so that you can download the current users leaderboard data for all leaderboard_names

### Submit New Leaderboard Entry

Add a leaderboard entry to a specific leaderboard within your game designated by 'leaderboard_name', these can be pulled later and compared to other users.

```
https://gamefuse.co/api/v2/users/{user id}/add_leaderboard_entry?score={score for the leaderboard}&leaderboard_name={leaderboard name}&extra_attributes={hash of extra data to save to the leaderboard entry}

```

It will have the following URL parameters:

```
score: {score for the leaderboard}
leaderboard_name: {leaderboard name within game}
extra_attributes: {hash of custom data}

```

This will return a response containing all the users attributes (custom data) with the following json:

```
{
  "id": {users id},
  "username": {users display username},
  "email": {system email - a combination of game id and email},
  "display_email": {users actual email, used for notifications and login}
  "credits": {number of credits user has, these can be used in your in game store},
  "score": {a generic score metric that you can use},
  "last_login": {timestamp of last login},
  "number_of_logins": {total logins},
  "authentication_token": {authentication token, this must be saved and added as a parameter to all authenticated requests},
  "events_total": {running api hits for this user},
  "events_current_month": {running api hits for this user for this month},
  "game_sessions_total": {unique game session for this user},
  "game_sessions_current_month": {unique game session for this user for this month}
}

```

```
* 401 - "can only add entries for current user"
* 400 - "invalid extra attributes"
* 500 - unknown server error

```

### Clear Leaderboard Entries

Clear all leaderboard entries for a specific user.

```
https://gamefuse.co/api/v2/users/{user id}/clear_my_leaderboard_entries?

```

It will have the following Headers:

```
authentication_token: {found in sign_in or sign_up response, authentication token for user session}

```

This will return a response containing all the users attributes (custom data) with the following json:

```
{
  "id": {users id},
  "username": {users display username},
  "email": {system email - a combination of game id and email},
  "display_email": {users actual email, used for notifications and login}
  "credits": {number of credits user has, these can be used in your in game store},
  "score": {a generic score metric that you can use},
  "last_login": {timestamp of last login},
  "number_of_logins": {total logins},
  "authentication_token": {authentication token, this must be saved and added as a parameter to all authenticated requests},
  "events_total": {running api hits for this user},
  "events_current_month": {running api hits for this user for this month},
  "game_sessions_total": {unique game session for this user},
  "game_sessions_current_month": {unique game session for this user for this month}
}

```

```
* 401 - "can only clear entries for current user"
* 500 - unknown server error

```

## Get All Leaderboard Entries

Get all leaderboard entries for a specific leaderboard name. You may have multiple leaderboards in your game, indicated by "leaderboard name"

```
https://gamefuse.co/api/v2/games/1/leaderboard_entries?leaderboard_name={leaderboard name}

```

It will have the following Headers:

```
authentication_token: {found in sign_in or sign_up response, authentication token for user session}

```

It will have the following URL parameters:

```
leaderboard_name: {name of the leaderboard within the game}

```

This will return a response containing all the users attributes (custom data) with the following json:

```
{
  "leaderboard_entries": [
      {
          "username": value,
          "score": value,
          "leaderboard_name": value,
          "game_user_id": value,
          "extra_attributes": value,
      },
      {
          "username": value,
          "score": value,
          "leaderboard_name": value,
          "game_user_id": value,
          "extra_attributes": value
      },
      ...
  ]
}

```

```
* 404 - No entries for this leaderboard name ___
* 500 - unknown server error

```

## Get My Leaderboard Entries

Get all leaderboard entries for a specific user.

```
https://gamefuse.co/api/v2/users/{user id}/leaderboard_entries?

```

It will have the following Headers:

```
authentication_token: {found in sign_in or sign_up response, authentication token for user session}

```

This will return a response containing all the users attributes (custom data) with the following json:

```
{
  "leaderboard_entries": [
      {
          "username": value,
          "score": value,
          "leaderboard_name": value,
          "game_user_id": value,
          "extra_attributes": value
      },
      {
          "username": value,
          "score": value,
          "leaderboard_name": value,
          "game_user_id": value,
          "extra_attributes": value
      },
      ...
  ]
}

```

```
* 401 - "can only get entries for current user"
* 500 - unknown server hasError
```