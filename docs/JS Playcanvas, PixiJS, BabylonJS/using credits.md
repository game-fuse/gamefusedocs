# Using Credits

Credits are a numeric attribute of each game user. It is a simple integer value
that you can add manually.

Credits are automatically detracted upon store item purchases.

What you find in the example below is a script that demonstrates the full
lifecycle of credits of a signed-in user. The `Start` function executes
these operations in order:

1. prints the credits your signed in user has
2. prints the cost of the first store item
3. adds credits to your user

Because the third operation syncs with the server it requires a callback.

Upon success you will see that the user now has more credits when logged.

Finally you can run the *purchase store item* function successfully.

!!!example
    ```jsx
    Start()
    {
        let self = this;
        console.log(GameFuseUser.CurrentUser.getCredits());  // Prints 0
        console.log(GameFuse.getStoreItems()[0].cost) // Prints 25 (or whatever you set your first item to on the web dashboard)
        GameFuseUser.CurrentUser.AddCredits(50, function(message,hasError){self.AddCreditsCallback(message,hasError)});
    }

    AddCreditsCallback(message, hasError)
    {
        if (hasError)
        {
            console.log("Error adding credits: " + message);
        }
        else
        {
            let self = this;
            console.log(GameFuseUser.CurrentUser.getCredits();  // Prints 50.
            GameFuseUser.PurchaseStoreItem(GameFuse.GetStoreItems()[0], function(message,hasError){self.PurchasedItemCallback(message,hasError)})
        }
    }

    PurchasedItemCallback(message, hasError)
    {
        if (hasError)
        {
            console.log("Error purchasing item: "+message);
        }
        else
        {
            console.log("Purchased Item");
            console.log("Current Credits: " + GameFuseUser.CurrentUser.GetCredits());
        }
    }
    ```

## Function return values

### `GameFuseUser.CurrentUser.AddCredits`

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK |
| `400`            | Credits parameter missing |
| `500`            | Unknown server error |
