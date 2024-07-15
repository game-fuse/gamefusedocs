# Signing game users up

Enable users to sign up in your Unity game with the following code. They will be saved on your GameFuse Game and can then login from other devices since the data is saved online. Add a method on a script on your sign up scene after you have collected your inputted username and password. Maybe this is on a a button function for a 'submit' or 'register' button. Username is mandatory but it is just for display. Later sign in attempts will use email not username

```jsx
#Feed in your users email, username and password here
signUp (email, password, password_confirmation, username) {
    #5th parameter is the callback when execution is complete
    let self = this;
    GameFuse.signUp(this.userEmail, "password", "password", this.username, function(message,hasError){self.signedUp(message,hasError)});
}

signedUp(message, hasError) {
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

```
* 404 - Failed to fetch game variables, check your token and id
* 500 - unknown server error
```