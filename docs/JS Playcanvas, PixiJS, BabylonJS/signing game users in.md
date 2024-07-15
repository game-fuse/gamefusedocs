# Signing game users in

Signing In Follows the same protocal as signing up, just with different parateters. As always, there is a callback function to let you know your sign in has been successful or not. Email and password (not username), will be used to sign in

```jsx
#Feed in your users email and password here
signIn (email, password, SignedIn) {
    #3rd parameter is the callback when execution is complete
    let self = this;
    GameFuse.signIn(this.userEmail, "password", function(message,hasError){self.signedIn(message,hasError)});
}

signedIn(message, hasError) {
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

```
# 404 - Incorrect Password
# 404 - User Not Found!
# 402 - Game is disabled - developer check GameFuse dashboard
* 500 - unknown server error
```