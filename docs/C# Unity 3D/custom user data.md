Custom user data or Key Value pairs are a simple way to save any kind of data for a particular user. Some examples might be {"world_2_unlocked":"true"}, {"player_color","red"}, {"favorite_food","Onion"} These are downloaded to your system upon login, and synced when one is updated. You can access with GameFuseUser.CurrentUser.attributes

All values and keys must be strings. If you want to use other data structures like arrays, you could stringify the array on save, and convert the saved string to an array on load:

```csharp
void Start(){
    Debug.Log(GameFuseUser.CurrentUser.attributes.Count);  // Prints 0
    Debug.Log(GameFuseUser.CurrentUser.GetAttributeValue("CURRENT_LEVEL") == null); // Prints true
    GameFuseUser.CurrentUser.SetAttribute("CURRENT_LEVEL", "5", SetAttributeCallback);
}

void SetAttributeCallback(string message, bool hasError) {
  if (hasError)
  {
      Debug.Log("Error setting attribute: "+message);
  }
  else
  {
      Debug.Log(GameFuseUser.CurrentUser.GetAttributeValue("CURRENT_LEVEL")); // Prints "5"
  }
}

```

You can also batch update a custom dictionary of key value pairs like this, or set local attributes then sync them all at once later. Dirty Attributes can be checked at any time to see which keys are not synced with the database.

```csharp
void Start(){
    Debug.Log(GameFuseUser.CurrentUser.attributes.Count);  // Prints 0
    Dictionary < string, string > attributesToUpdate = new Dictionary < string, string >{
        { "POINTS", "1000" },
        { "LEVEL", "5" },
        { "CHARACTER", "Ninja" }
    };
    GameFuseUser.CurrentUser.SetAttributes(attributesToUpdate, SetAttributesCallback);
}

void SetAttributesCallback(string message, bool hasError) {
  if (hasError)
  {
      Debug.Log("Error setting attribute: "+message);
  }
  else
  {
      Debug.Log("Batch update complete");
      Debug.Log(GameFuseUser.CurrentUser.GetAttributeValue("POINTS")); // Prints "1000"
      Debug.Log(GameFuseUser.CurrentUser.GetAttributeValue("LEVEL")); // Prints "5"
      GameFuseUser.CurrentUser.SetAttributeLocal("POINTS", "2000"); //will set locally but not sync with db
      GameFuseUser.CurrentUser.SetAttributeLocal("LEVEL", "6"); //will set locally but not sync with db
      Debug.Log("Dirty Vars");  //will print variables changed locally
      foreach (var attribute in GameFuseUser.CurrentUser.GetDirtyAttributes()){
        Debug.Log(attribute.Key+": "+attribute.Value);
      }

      GameFuseUser.CurrentUser.SyncLocalAttributes(AttributesSynced); //will update "SCORE" + "LEVEL"
  }
}
void AttributesSynced(string message, bool hasError){
  if (hasError)
  {
      Debug.Log("Error syncing attributes: "+message);
  }
  else
  {
      Debug.Log("Sync Success");
      Debug.Log("Dirty Vars"); //will be empty now that sync is done
      foreach (var attribute in GameFuseUser.CurrentUser.GetDirtyAttributes()){
        Debug.Log(attribute.Key+": "+attribute.Value);
      }
  }
}

```

```
* 400 - each attribute a 'key' and 'value' parameter
* 400 - missing or invalid parameters
* 500 - unknown server error
```