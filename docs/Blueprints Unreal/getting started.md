# Getting Started with GameFuse Blueprints

The first step of integrating GameFuse with your project is to make an account.

[Sign Up](https://gamefuse.co/users/sign_up){ .md-button }

After creating your account, add your first game and note the ID and API Token.

With this setup, you can now connect via your game client.

## Installation

1. Download the Plugin from GitHub or the Unreal Store
2. Unzip the code
3. Add it to your *UnrealEngine* project in the *Plugins* folder
4. Enable the plugin inside the engine

<div class="grid cards" markdown>
- [Fab Store](https://www.fab.com/listings/b541509e-d123-4785-b26e-ac2a3ed89ea1){ .md-button }
- [Unreal Plugin Source](https://github.com/game-fuse/game-fuse-cpp){ .md-button }
</div>

## Quick Start Example

Here's a basic example of how to set up GameFuse in your game using Blueprints:

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/k0hc79fl/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

## Key Changes in V2.9

GameFuse V2.9 introduced significant architectural changes:

- **Subsystem Architecture**: All GameFuse functionality is now provided through Unreal Engine subsystems
- **Callback Events**: Replaced latent nodes with callback events for better performance
- **Struct-Based Data**: Replaced UObjects with structs for better performance and memory usage
- **Request Tracking**: All API calls return a unique request ID for tracking

For detailed migration information, see the [V2.9 Migration Guide](V2.9%20Blueprint%20Migration%20Guide.md).

## Example Project

Click on the following link to see an example of GameFuse implemented in Unreal 5.4.4:

[Unreal Example](https://github.com/game-fuse/game-fuse-unreal-example){ .md-button }

## Next Steps

- [Sign Up Users](signing%20game%20users%20up.md) - Learn how to create user accounts
- [Sign In Users](signing%20game%20users%20in.md) - Learn how to authenticate users
- [Class Methods](class%20methods.md) - Reference for all available methods
- [Callback Reference](callback%20reference.md) - Learn about the callback system
