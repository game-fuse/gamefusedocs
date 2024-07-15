Use GameFuse C# in your Unity project to easily add authentiction, user data, leader boards, in game stores and more all without writing and hosting any servers, or writing any API code. Its never been easier to store your data online! GameFuse is always free for hobby and small indie projects. With larger usage there are metered usage fees.

## Getting Started

The first step of integrating GameFuse with your project, is to make an account

- **[Sign Up](https://gamefuse.co/users/sign_up)**

After creating your account, add your first game and note the ID and API Token.

With this setup, you can now connect via your game client. Download the library and unzip the code from the Github, then add to your Unity project in the Scripts folder.

- **[C# Library](https://github.com/game-fuse/game-fuse-cs)**

If you would like to see an example of how the code works, check out

- **[Unity Script Example](https://github.com/game-fuse/game-fuse-unity-example)**

At this point in time, you would add the prefab in this folder GameFuseInitializer

You can also build this manually by adding new GameObject to your first scene, and add a number of script componenets:

- GameFuse.cs
- GameFuseLeaderboardEntry.cs
- GameFuseUser.cs
- GameFuseUtilities.cs
- GameFuseStoreItem.cs

At this point in time you have the library installed in your game, and you are ready to connect.