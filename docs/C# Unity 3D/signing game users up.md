# Signing game users up

Enable users to sign up in your Unity game with the following code. They will be saved on your GameFuse Game and can then login from other devices since the data is saved online. Add a method on a MonoBehavior on your sign up scene after you have collected your inputted username and password. Maybe this is on a a button function for a 'submit' or 'register' button. Username is mandatory but it is just for display. Later sign in attempts will use email not username

```csharp
#Feed in your users email, username and password here
void SignUp (email, password, password_confirmation, username) {
    #5th parameter is the callback when execution is complete
    GameFuse.SignUp(userEmail, "password", "password", username, SignedUp);
}

void SignedUp(string message, bool hasError) {
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

```
* 404 - Failed to fetch game variables, check your token and id
* 500 - unknown server error
```