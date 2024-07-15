
After sign up, you should provide a sign in function for users to login later

```
https://gamefuse.co/api/v2/sessions?email={email}&password={password}&game_id={game id}&game_token={api token}

```

It will have the following URL parameters:

```
email: {users email, used for login and forgot password functionality}
password: {users password}
game_id: {found on your GameFuse.co dashboard}
game_token: {API token found on your GameFuse.co dashboard}

```

This will return a response with the following json:

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
# 404 - Incorrect Password
# 404 - User Not Found!
# 402 - Game is disabled - developer check GameFuse dashboard
* 500 - unknown server error
```