# Using the store in your game

Store items are downloaded upon `SignIn()` and `SignUp()`. The items will be
refreshed every time the user signs in or signs up again.

To access store items and attributes call the following code. This code does
not sync them with the available items on the server: it simply shows you the
results downloaded on sign-in or sign-up.

!!! example
    ```jsx
    for (const storeItem of GameFuse.getStoreItems())
    {
        console.log(storeItem.getName());  // FireBow
        console.log(storeItem.getCategory()); // BowAndArrows
        console.log(storeItem.getId()); //12
        console.log(storeItem.getDescription());  // A bow and arrow item that shoots fire arrows
        console.log(storeItem.getCost()); // 500 (credits)
        console.log(storeItem.getIconUrl()); // (url of image associated with the store item)
    }
    ```

To access purchased store items by your current logged in user, call the
following code. Because these items are downloaded on login, there is no
callback for this: data is already available. This code will throw an error if
you are not signed in already.

```jsx
const items = GameFuseUser.CurrentUser.getPurchasedStoreItems();
```

To purchase a store item simply call the code below. Because this function talks
to the server, it will require a callback. If the user does not have enough
credits on their account (see next section), the purchase will fail. This
function will refresh the `GameFuseUser.CurrentUser.purchasedStoreItems` list
with the new item.

!!! example
    ```jsx
    PurchaseItem(store_item)
    {
        let self = this;
        console.log(GameFuseUser.CurrentUser.getPurchasedStoreItems().length); // Prints 0
        GameFuseUser.PurchaseStoreItem(GameFuse.GetStoreItems().First, function(message,hasError){self.PurchasedItemCallback(message,hasError)})
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
            console.log(GameFuseUser.CurrentUser.getPurchasedStoreItems().length); // Prints 1
        }
    }
    ```

## Function return values

### `GameFuseUser.PurchaseStoreItem`

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK |
| `403`            | Not enough credits or item already purchased |
| `404`            | Item not found |
| `500`            | Unknown server error |
