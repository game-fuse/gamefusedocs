# Custom user data

Custom user data or key-value pairs are a simple way to save any kind of data
for a specific user. An example might be:

```plaintext
{"world_2_unlocked":"true"} {"player_color","red"}, {"favorite_food","Onion"}
```

These are downloaded to your system upon login and synced when one is updated.

All values and keys must be strings. If you want to use other data structures
like arrays, you could stringify the array while saving. When loading the data
you must then convert the saved string into an array.

<iframe src="https://blueprintue.com/render/9e0uryhw/" width="800" height="600" frameborder="0" allowfullscreen></iframe>
