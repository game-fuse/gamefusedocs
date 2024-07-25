# Using the store in your game

Store items are fetched upon sign-in and sign-up. The items will be
refreshed every time the user signs in or signs up again.

To access store items and attributes call the following node. This node does
not sync them with the available items on the server: it simply shows you the
results downloaded on sign-in or sign-up.

<iframe src="https://blueprintue.com/render/tglhpyfa/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

To access purchased store items by your current logged in user, call the
following node. Because these items are downloaded on login, there is no
callback for this: data is already available. This node will throw an error if
you are not signed in already.

<iframe src="https://blueprintue.com/render/o0353ou7/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

To purchase a store item simply call the node below. Because this node talks to
the server, it will require a callback. If the user does not have enough
credits on their account (see next section), the purchase will fail. This
function will refresh the `Purchased Store Items` list with the new item.

<iframe src="https://blueprintue.com/render/7b7ykq81/" width="800" height="600" frameborder="0" allowfullscreen></iframe>
