# Using Credits

Credits are a numeric attribute of each game user. It is a simple integer value
that you can add manually.

Credits are automatically detracted upon store item purchases.

What you find in the example below is a script that demonstrates the full
lifecycle of credits of a signed-in user.  the `Start` function executes
these operations in order:

1. prints the credits your signed in user has
2. prints the cost of the first store item
3. adds credits to your user

Because the third operation syncs with the server it requires a callback.

Upon success you will see that the user now has more credits when logged. 

Finally you can run the *purchase store item* function successfully.

!!! example
    ```csharp
    void Start()
    {
        Debug.Log(GameFuseUser.CurrentUser.credits);  // Prints 0
        Debug.Log(GameFuse.GetStoreItems().First.cost) // Prints 25 (or whatever you set your first item to on the web dashboard)
        GameFuseUser.CurrentUser.AddCredits(50, AddCreditsCallback);
    }

    void AddCreditsCallback(string message, bool hasError)
    {
        if (hasError)
        {
            Debug.Log("Error adding credits: " + message);
        }
        else
        {
            Debug.Log(GameFuseUser.CurrentUser.credits;  // Prints 50
            GameFuseUser.PurchaseStoreItem(GameFuse.GetStoreItems().First, PurchasedItemCallback)
        }

    }

    void PurchasedItemCallback(string message, bool hasError)
    {
        if (hasError)
        {
            Debug.Log("Error purchasing item: "+message);
        }
        else
        {
            Debug.Log("Purchased Item");
            Debug.Log("Current Credits: " + GameFuseUser.CurrentUser.GetCredits());
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
