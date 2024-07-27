Custom user data or key-value pairs are a simple way to save any kind of data
for a specific user. An example might be:

```plaintext
{"world_2_unlocked":"true"} {"player_color","red"}, {"favorite_food","Onion"}
```

These are downloaded to your system upon login and synced when one is updated.

All values and keys must be strings. If you want to use other data structures
like arrays, you could stringify the array while saving. When loading the data
you must then convert the saved string into an array.

!!! example
    ```csharp
    void Start() {
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

You can also batch update a custom dictionary of key value pairs or
set local attributes then sync them all at once later. *Dirty Attributes* can
be checked at any time to see which keys are not synced with the database.

!!! example
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
          foreach (var attribute in GameFuseUser.CurrentUser.GetDirtyAttributes()) {
            Debug.Log(attribute.Key+": "+attribute.Value);
          }

          GameFuseUser.CurrentUser.SyncLocalAttributes(AttributesSynced); //will update "SCORE" + "LEVEL"
      }
    }

    void AttributesSynced(string message, bool hasError) {
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

## Function return values

### `GameFuseUser.CurrentUser.SetAttributes`

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK |
| `400`            | Missing or invalid parameters, or some attribute is missing a `key` or `value` parameter |
| `500`            | Unknown server error |
