User data can be set in a number of ways. Score can be set with a specific API call to set or relativly add to score. You can also set custom user data where you can assign any key to any value. Whether you use it for a players "current_level", "color", "XP" or anything else you can think of, it can be done with the custom data.

## Adding scores

To alter the amount of scores a users has relativly, use:

```
https://gamefuse.co/api/v2/users/{signed in user id}/add_scores?&scores={scores to add}

```

It will have the following Headers:

```
authentication_token: {found in sign_in or sign_up response, authentication token for user session}

```

It will have the following URL parameters:

```
scores: {the amount of scores positive or negative you want to alter the users current scores by}

```

This will return a response with the following json. It has users remaining scores, and a list of all their purchased store items

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
* 400 - Please include a score param
* 500 - unknown server error

```

## Setting scores

To set the amount of scores a users has absolutly, meaning the scores param will be the users new scores total, use:

```
https://gamefuse.co/api/v2/users/{signed in user id}/set_scores?&scores={scores to add}

```

It will have the following Headers:

```
authentication_token: {found in sign_in or sign_up response, authentication token for user session}

```

It will have the following URL parameters:

```
scores: {the amount of scores a user will now have}

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
* 400 - Please include a score param
* 500 - unknown server error

```

## Adding Custom Attribute (Custom Data)

Set any key to any value. It will be in a string format but can be converted in your programs language to any type.

```
https://gamefuse.co/api/v2/users/{signed in user id}/add_game_user_attribute?&key={key to add}&value={value to add}&attributes={json of attributes defined below}

```

It will have the following Headers:

```
authentication_token: {found in sign_in or sign_up response, authentication token for user session}

```

It will have the following URL parameters:

```
key: {the key of the datum you are trying to save}
value: {the value of the datum you are trying to save}
attributes: {json of the format [{"key": key1, "value": value1}, {"key": key2, "value":value2}...] for batch updating }

```

This will return a response containing all the users attributes (custom data) with the following json:

```
{
  "game_user_attributes": [
      {
          "id": {id of first attribute},
          "key": {key of first attribute},
          "value": {value of first attribute}
      },
      {
          "id": {id of second attribute},
          "key": {key of second attribute},
          "value": {value of second attribute}
      },
      ...
  ]
}

```

```
* 400 - each attribute a 'key' and 'value' parameter
* 400 - missing or invalid parameters
* 500 - unknown server error

```

## Removing Custom Attribute (Custom Data)

remove any key and its value

```
https://gamefuse.co/api/v2/users/{signed in user id}/remove_game_user_attributes?&key={key to add}

```

It will have the following Headers:

```
authentication_token: {found in sign_in or sign_up response, authentication token for user session}

```

It will have the following URL parameters:

```
key: {the key of the datum you are trying to remove}

```

This will return a response containing all the users attributes (custom data) with the following json:

```
{
  "game_user_attributes": [
      {
          "id": {id of first attribute},
          "key": {key of first attribute},
          "value": {value of first attribute}
      },
      {
          "id": {id of second attribute},
          "key": {key of second attribute},
          "value": {value of second attribute}
      },
      ...
  ]
}

```

```
* 400 - User does not have item with specified key
* 500 - unknown server error

```

## Get All Custom Attribute (Custom Data)

Get a list of all the signed in users custom attributes

```
https://gamefuse.co/api/v2/users/{signed in user id}/game_user_attributes?

```

It will have the following Headers:

```
authentication_token: {found in sign_in or sign_up response, authentication token for user session}

```

This will return a response containing all the users attributes (custom data) with the following json:

```
{
  "game_user_attributes": [
      {
          "id": {id of first attribute},
          "key": {key of first attribute},
          "value": {value of first attribute}
      },
      {
          "id": {id of second attribute},
          "key": {key of second attribute},
          "value": {value of second attribute}
      },
      ...
  ]
}

```

```
* 500 - unknown server error
```