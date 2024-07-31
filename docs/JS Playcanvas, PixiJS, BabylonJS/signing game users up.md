# Signing game users up

Enable users to sign up in your Unity game with the following code.
These users will be saved in your GameFuse game and can then login from other
devices, since the data is saved online.

Add a method on a script in your sign-up scene after you have collected 
the inputted `username` and `password`. You can create a function connected     
to buttons for submit and register operations.

The `username` parameter is mandatory but it is just for display. Later sign-in 
attempts will use the `email`, not the `username`.

!!! example
    ```jsx
    // Feed in your users email, username and password here.
    signUp (email, password, password_confirmation, username)
    {
        // The fith parameter is the callback when execution is complete.
        let self = this;
        GameFuse.signUp(this.userEmail, "password", "password", this.username, function(message,hasError){self.signedUp(message,hasError)});
    }

    signedUp(message, hasError)
    {
        if (hasError)
        {
            console.log("Error signign up: "+message);
        }
        else
        {
            console.log("Signed Up: " + GameFuseUser.CurrentUser.getUsername());
        }
    }
    ```

## Function return values

### `GameFuse.signUp`

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK |
| `404`            | Failed to fetch game variables. Check your token and Game ID |
| `500`            | Unknown server error |
