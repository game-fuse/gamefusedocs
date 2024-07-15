Credits are a numeric attribute of each game user. It is a simple integer value. You can add them manually and they are detracted automatically upon store item purchases. You can manually add and remove them via an API call, in addition to purchasing store items with them.

## Adding Credits

To alter the amount of credits a users has relativly, use:

```
https://gamefuse.co/api/v2/users/{signed in user id}/add_credits?credits={credits to add}

```

It will have the following Headers:

```
authentication_token: {found in sign_in or sign_up response, authentication token for user session}

```

It will have the following URL parameters:

```
credits: {the amount of credits positive or negative you want to alter the users current credits by}

```

This will return a response with the following json. It has users remaining credits, and a list of all their purchased store items

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
* 400 - Please include a credits param
* 500 - unknown server error

```

## Setting Credits

To set the amount of credits a users has absolutly, meaning the credits param will be the users new credits total, use:

```
https://gamefuse.co/api/v2/users/{signed in user id}/set_credits?credits={credits to add}

```

It will have the following Headers:

```
authentication_token: {found in sign_in or sign_up response, authentication token for user session}

```

It will have the following URL parameters:

```
credits: {the amount of credits a user will now have}

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
* 400 - Please include a credits param
* 500 - unknown server error
```