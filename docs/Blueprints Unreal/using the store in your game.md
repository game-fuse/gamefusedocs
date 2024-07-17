# Using the store in your game

Store Items Library are fetched upon SignIn and SignUp, The Items will be refreshed every time the user signs in or signs up again. To access store items and attributes by calling the following node. This doesn't sync them with the available items on the server, it is simply showing you the results downloaded on sign in or sign up.

<iframe src="https://blueprintue.com/render/tglhpyfa/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

To access purchased store items by your current logged in user call the following. Because these are downloaded on login, there is no callback for this! It is already available! This will throw an error if you are not signed in already.

<iframe src="https://blueprintue.com/render/o0353ou7/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

To Purchase a store item simply call the node below. Because this node talks to the server, it will require a callback. If the user doesn't have enough credits on their account (see next section), the purchase will fail. This function will refresh the Purchased Store Items List with the new item.

<iframe src="https://blueprintue.com/render/7b7ykq81/" width="800" height="600" frameborder="0" allowfullscreen></iframe>