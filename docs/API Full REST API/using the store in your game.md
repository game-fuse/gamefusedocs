## Getting Available Store Items

To grab all your store items available on your store you can call:

```
https://gamefuse.co/api/v2/games/store_items?game_id={game id}&game_token={api token}

```

It will have the following URL parameters:

```
game_id: {found on your GameFuse.co dashboard}
game_token: {API token found on your GameFuse.co dashboard}

```

This will return a response with the following json:

```
{
  "store_items": [
      {
          "id": {first item id},
          "name": {first item name},
          "cost": {first item cost},
          "description": {first item description},
          "category": {first item category},
          "icon_url": {linked image url for this store item}
      },
      {
          "id": {second item id},
          "name": {second item name},
          "cost": {second item cost},
          "description": {second item description},
          "category": {second item category},
          "icon_url": {linked image url for this store item}
      },
      ...
  ]
}

```

This is a non-authenticated request, a user doesnt need to be signed in since there is no user specific data

```
* 404 - Failed to verify game
* 500 - unknown server error

```

## Purchasing a Store Item

To purchase a store item, a user must be signed in and have enough credits. This is our first authenticated request and must have an 'access_token' parameter recieved from a sign_in or sign_up response.

```
https://gamefuse.co/api/v2/users/{signed in user id}/purchase_game_user_store_item?store_item_id={id of store item}

```

It will have the following Headers:

```
authentication_token: {found in sign_in or sign_up response, authentication token for user session}

```

It will have the following URL parameters:

```
store_item_id: {item id of store item}

```

This will return a response with the following json. It has users remaining credits, and a list of all their purchased store items

```
{
  "credits": {users remaining credits},
  "game_user_store_items": [
      {
          "id": {purchased item 1 id},
          "name": {purchased item 1 name},
          "cost": {purchased item 1 cost},
          "description": "{purchased item 1 description},
          "category": {purchased item 1 category},
          "icon_url": {linked image url for purchased item 1}
      },
      {
          "id": {purchased item 2 id},
          "name": {purchased item 2 name},
          "cost": {purchased item 2 cost},
          "description": "{purchased item 2 description},
          "category": {purchased item 2 category},
          "icon_url": {linked image url for purchased item 2}
      },
      ...
  ]
}

```

```
* 403 - Already Purchased
* 403 - Not Enough Credits
* 404 - Item not found
* 500 - unknown server error

```

## Removing a Store Item

To revoke a store item purchase, a user must be signed in.

```
https://gamefuse.co/api/v2/users/{signed in user id}/remove_game_user_store_item?store_item_id={id of store item to delete}

```

It will have the following Headers:

```
authentication_token: {found in sign_in or sign_up response, authentication token for user session}

```

It will have the following URL parameters:

```
store_item_id: {item id of store item to delete}

```

This will return a response with the following json. It has users remaining credits, and a list of all their purchased store items

```
{
  "credits": {users remaining credits},
  "game_user_store_items": [
      {
          "id": {purchased item 1 id},
          "name": {purchased item 1 name},
          "cost": {purchased item 1 cost},
          "description": "{purchased item 1 description},
          "category": {purchased item 1 category},
          "icon_url": {linked image url for purchased item 1}
      },
      {
          "id": {purchased item 2 id},
          "name": {purchased item 2 name},
          "cost": {purchased item 2 cost},
          "description": "{purchased item 2 description},
          "category": {purchased item 2 category},
          "icon_url": {linked image url for purchased item 2}
      },
      ...
  ]
}

```

```
* 404 - Store item does not exist, or was not purchase
* 500 - unknown server error

```

## Getting Purchased Store Items

To check out all of the users purchased store items

```
https://gamefuse.co/api/v2/users/{signed in user id}/game_user_store_items

```

It will have the following Headers:

```
authentication_token: {found in sign_in or sign_up response, authentication token for user session}

```

This will return a response with the following json. It has users remaining credits, and a list of all their purchased store items

```
{
  "credits": {users remaining credits},
  "game_user_store_items": [
      {
          "id": {purchased item 1 id},
          "name": {purchased item 1 name},
          "cost": {purchased item 1 cost},
          "description": "{purchased item 1 description},
          "category": {purchased item 1 category},
          "icon_url": {linked image url for purchased item 1}
      },
      {
          "id": {purchased item 2 id},
          "name": {purchased item 2 name},
          "cost": {purchased item 2 cost},
          "description": "{purchased item 2 description},
          "category": {purchased item 2 category},
          "icon_url": {linked image url for purchased item 2}
      },
      ...
  ]
}

```

```
* 500 - unknown server error
```