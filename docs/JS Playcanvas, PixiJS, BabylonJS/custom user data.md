# Custom user data

Custom user data or Key Value pairs are a simple way to save any kind of data for a particular user. Some examples might be {"world_2_unlocked":"true"}, {"player_color","red"}, {"favorite_food","Onion"} These are downloaded to your system upon login, and synced when one is updated. You can access with GameFuseUser.CurrentUser.attributes

All values and keys must be strings. If you want to use other data structures like arrays, you could stringify the array on save, and convert the saved string to an array on load.

```jsx
Start(){
    let self = this;
    console.log(GameFuseUser.CurrentUser.attributes.length);  // Prints 0
    console.log(GameFuseUser.CurrentUser.GetAttributeValue("CURRENT_LEVEL") == null); // Prints true
    GameFuseUser.CurrentUser.SetAttribute("CURRENT_LEVEL", "5", function(message,hasError){self.SetAttributeCallback(message,hasError)});
}

SetAttributeCallback(message, hasError) {
  if (hasError)
  {
      console.log("Error setting attribute: "+message);
  }
  else
  {
      console.log(GameFuseUser.CurrentUser.GetAttributeValue("CURRENT_LEVEL")); // Prints "5"
  }
}

```

ou can also batch update a custom dictionary of key value pairs like this, or set local attributes then sync them all at once later. Dirty Attributes can be checked at any time to see which keys are not synced with the database.

```jsx
Start(){
    console.log(GameFuseUser.CurrentUser.attributes.Count);  // Prints 0
    attributesToUpdate = {{ "POINTS": "1000" },{ "LEVEL": "5" },{ "CHARACTER": "Ninja" }};
    GameFuseUser.CurrentUser.setAttributes(attributesToUpdate, function(message,hasError){self.setAttributesCallback(message,hasError)});
}

setAttributesCallback(message, hasError) {
  if (hasError)
  {
      console.log("Error setting attribute: "+message);
  }
  else
  {
      console.log("Batch update complete");
      console.log(GameFuseUser.CurrentUser.getAttributeValue("POINTS")); // Prints "1000"
      console.log(GameFuseUser.CurrentUser.getAttributeValue("LEVEL")); // Prints "5"
      GameFuseUser.CurrentUser.setAttributeLocal("POINTS", "2000"); //will set locally but not sync with db
      GameFuseUser.CurrentUser.setAttributeLocal("LEVEL", "6"); //will set locally but not sync with db
      console.log("Dirty Vars");  //will print variables changed locally
      console.log(GameFuseUser.CurrentUser.getDirtyAttributes());
      GameFuseUser.CurrentUser.syncLocalAttributes(function(message,hasError){self.attributesSynced(message,hasError)}); //will update "SCORE" + "LEVEL"
  }
}
attributesSynced(message, hasError){
  if (hasError)
  {
      console.log("Error syncing attributes: "+message);
  }
  else
  {
      console.log("Sync Success");
      console.log("Dirty Vars"); //will be empty now that sync is done
      console.log(GameFuseUser.CurrentUser.getDirtyAttributes());
  }
}

```

```
* 400 - each attribute a 'key' and 'value' parameter
* 400 - missing or invalid parameters
* 500 - unknown server error
```