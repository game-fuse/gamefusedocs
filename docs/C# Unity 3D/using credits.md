# Using Credits

Credits are a numeric attribute of each game user. It is a simple integer value. You can add them manually and they are detracted automatically upon store item purchases Below is a script to demonstrate the full lifecycle of credits on a signed in user. First it prints the credits your signed in user has, then prints the cost of the first store item, then it adds credits to your user. Because this syncs with the server, it requires a callback. Upon success, you will see the user now has more credits when logged. At this point in time you can then run the purchase store item function successfully.

```csharp
void Start(){
    Debug.Log(GameFuseUser.CurrentUser.credits;  // Prints 0
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

void PurchasedItemCallback(string message, bool hasError) {
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

```
* 400 - Please include a credits param
* 500 - unknown server error
```


