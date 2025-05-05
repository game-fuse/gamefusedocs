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

### Installing the SDK

You can easily add GameFuse to your Unity project by editing your project's `manifest.json` file. Add the following line to your dependencies:

```json
"com.gamefuse.sdk": "https://github.com/game-fuse/game-fuse-cs.git?path=com.gamefuse.sdk#v3api"
```

Unity will automatically import the package for you.

[C# Library](https://github.com/game-fuse/game-fuse-cs){ .md-button }

### Using the Sample

The GameFuse SDK includes a sample to help you get started quickly. You can import this sample through the Unity Package Manager after the SDK is installed.

**Note:** To run the samples, you will need to import Text Mesh Pro Essentials if you haven't already done so in your project. After importing TMP Essentials, you may need to restart Unity to avoid initialization errors with TextMeshPro components.
