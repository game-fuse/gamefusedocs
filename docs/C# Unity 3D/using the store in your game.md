# Using the store in your game

Store Items Library are downloaded upon SignIn() and SignUp(), The Items will be refreshed every time the user signs in or signs up again. To access store items and attributes by calling the following code. This doesnt sync them with the available items on the server, it is simply showing you the results downloaded on sign in or sign up.

```csharp
foreach (GameFuseStoreItem storeItem in GameFuse.GetStoreItems())
{
    Debug.Log(storeItem.GetName());  //FireBow
    Debug.Log(storeItem.GetCategory()); //BowAndArrows
    Debug.Log(storeItem.GetId()); //12
    Debug.Log(storeItem.GetDescription());  //A bow and arrow item that shoots fire arrows
    Debug.Log(storeItem.GetCost()); // 500 (credits)
    Debug.log(storeItem.GetIconUrl()); // (url of image associated with the store item)
}

```

To access purchased store items by your current logged in user call the following. Because these are downloaded on login, there is no callback for this! It is already available! This will throw an error if you are not signed in already

```csharp
List < GameFuseStoreItem > items = GameFuseUser.CurrentUser.GetPurchasedStoreItems();

```

To Purchase a store item simply call the code below. Because this function talks to the server, it will require a callback. If the user doesnt have enough credits on their account (see next section), the purchase will fail This function will refresh the GameFuseUser.CurrentUser.purchasedStoreItems List with the new item

```csharp
void PurchaseItem(store_item){
  Debug.Log(GameFuseUser.CurrentUser.purchasedStoreItems.Count); // Prints 0
  GameFuseUser.PurchaseStoreItem(GameFuse.GetStoreItems().First, PurchasedItemCallback)
}

void PurchasedItemCallback(string message, bool hasError) {
  if (hasError)
  {
      Debug.Log("Error purchasing item: "+message);
  }
  else
  {
      Debug.Log("Purchased Item");
      Debug.Log(GameFuseUser.CurrentUser.purchasedStoreItems.Count); // Prints 1
  }
}

```

```
* 500 - unknown server error
```
