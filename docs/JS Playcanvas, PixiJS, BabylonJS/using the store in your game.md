# Using the store in your game

Store Items Library are downloaded upon SignIn() and SignUp(), The Items will be refreshed every time the user signs in or signs up again. To access store items and attributes by calling the following code. This doesnt sync them with the available items on the server, it is simply showing you the results downloaded on sign in or sign up.

```jsx
for (const storeItem of GameFuse.getStoreItems()) {
    console.log(storeItem.getName());  //FireBow
    console.log(storeItem.getCategory()); //BowAndArrows
    console.log(storeItem.getId()); //12
    console.log(storeItem.getDescription());  //A bow and arrow item that shoots fire arrows
    console.log(storeItem.getCost()); // 500 (credits)
    console.log(storeItem.getIconUrl()); // (url of image associated with the store item)
}

```

To access purchased store items by your current logged in user call the following. Because these are downloaded on login, there is no callback for this! It is already available! This will throw an error if you are not signed in already

```jsx
const items = GameFuseUser.CurrentUser.getPurchasedStoreItems();
  %p.quickSand To Purchase a store item simply call the code below. Because this function talks to the server, it will require a callback. If the user doesnt have enough credits on their account (see next section), the purchase will fail This function will refresh the GameFuseUser.CurrentUser.purchasedStoreItems List with the new item

```

```jsx
PurchaseItem(store_item){
  let self = this;
  console.log(GameFuseUser.CurrentUser.getPurchasedStoreItems().length); // Prints 0
  GameFuseUser.PurchaseStoreItem(GameFuse.GetStoreItems().First, function(message,hasError){self.PurchasedItemCallback(message,hasError)})
}

PurchasedItemCallback(message, hasError) {
  if (hasError)
  {
      console.log("Error purchasing item: "+message);
  }
  else
  {
      console.log("Purchased Item");
      console.log(GameFuseUser.CurrentUser.getPurchasedStoreItems().length); // Prints 1
  }
}

```

```
* 500 - unknown server error
```