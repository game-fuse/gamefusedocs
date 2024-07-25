# Using Credits

Credits are a numeric attribute of each game user. It is a simple integer
value. You can add them manually and they are detracted automatically upon
store item purchases.

What follows is a script to demonstrate the full life cycle of credits on a
signed-in user:

1. prints the credits your signed-in user has
2. prints the cost of the first store item
3. adds credits to your user

Because this operation synchronizes with the server it requires a callback.
Upon success, you will see the user has now more credits when logged. At this
point in time, you can run the `Purchase Store Item` function successfully. 

<iframe src="https://blueprintue.com/render/g7vz1rtv/" width="800" height="600" frameborder="0" allowfullscreen></iframe>
