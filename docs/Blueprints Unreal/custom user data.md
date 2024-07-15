# Custom user data

Custom user data or Key Value pairs are a simple way to save any kind of data for a particular user. Some examples might be {"world_2_unlocked":"true"}, {"player_color","red"}, {"favorite_food","Onion"}. These are downloaded to your system upon login, and synced when one is updated. You can access with Get Attributes node like this:

All values and keys must be strings. If you want to use other data structures like arrays, you could stringify the array on save, and convert the saved string to an array on load: