Use GameFuse C# in your Unity project to easily add:

- authentiction
- user data
- leader boards
- in game stores
- and more

all without writing and hosting any servers, or writing any API code. It's
never been easier to store your data online!

GameFuse is always free for hobby and small indie projects. With larger usage
there are metered usage fees.

## Getting Started

The first step of integrating GameFuse with your project, is to make an account

[Sign Up](https://gamefuse.co/users/sign_up){ .md-button }

After creating your account, add your first game and note the ID and API Token.

With this setup, you can now connect via your game client.

1. download the Plugin from GitHub
2. unzip the code
3. add it to your Unity project in the *Scripts* folder

[C# Library](https://github.com/game-fuse/game-fuse-cs){ .md-button }

If you would like to see an example of how the code works, check out this:

[Unity Script Example](https://github.com/game-fuse/game-fuse-unity-example){ .md-button }

At this point you would add the prefab in the `GameFuseInitializer` directory.
You can also build this manually by adding a new `GameObject` to your first
scene. You can then add a number of script componenets, such as:

- `GameFuse.cs`
- `GameFuseLeaderboardEntry.cs`
- `GameFuseUser.cs`
- `GameFuseUtilities.cs`
- `GameFuseStoreItem.cs`

Now the library is installed in your game: you are ready to connect.
