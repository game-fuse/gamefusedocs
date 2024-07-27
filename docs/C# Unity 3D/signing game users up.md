# Signing game users up

Using the following code users will be able to sign up in your Unity game.
These users will be saved on your GameFuse Game and can login from other
devices because the data is saved online.

Add a method on a `MonoBehavior` in your sign-up scene after you have collected
the inputted `username` and `password`. You can create a function connected
to buttons for submit and register operations.

The `username` parameter is mandatory but it is just for display. Later sign-in
attempts will use the `email`, not the `username`.

!!! example
    ```csharp
    // Feed in your users email, username and password here.
    void SignUp (email, password, password_confirmation, username)
    {
        // The fith parameter is the callback when execution is complete.
        GameFuse.SignUp(userEmail, password, password_confirmation, username, SignedUp);
    }

    void SignedUp(string message, bool hasError)
    {
        if (hasError)
        {
            Debug.Log("Error signign up: "+message);
        }
        else
        {
            Debug.Log("Signed Up: " + GameFuseUser.CurrentUser.GetUsername());
        }
    }
    ```

## Function return values

### `GameFuse.SignUp`

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK |
| `404`            | Failed to fetch game variables. Check your token and Game ID |
| `500`            | Unknown server error |
