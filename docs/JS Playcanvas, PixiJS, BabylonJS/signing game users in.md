# Sign-in game users

The sign-in process follows the same rules as signing up, just with different
parameters. There is a callback function  to let you know your sign-in has
been successful or not.

Email and password, not the username, will be used to sign-in.

!!! example
    ```jsx
    // Feed in your users email and password here
    signIn (email, password, SignedIn)
    {
        // The third parameter is the callback when execution is complete.
        let self = this;
        GameFuse.signIn(this.userEmail, "password", function(message,hasError){self.signedIn(message,hasError)});
    }

    signedIn(message, hasError)
    {
        if (hasError)
        {
            console.log("Error signign in: "+message);
        }
        else
        {
            console.log("Logged In: " + GameFuseUser.CurrentUser.getUsername());
            console.log("Current Credits: " + GameFuseUser.CurrentUser.getCredits());
        }
    }
    ```

## Function return values

### `GameFuse.signIn`

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK |
| `402`            | Game is disabled (check the GameFuse dashboard) |
| `404`            | Incorrect password or user not found |
| `500`            | Unknown server error |
