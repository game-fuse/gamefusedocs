The GameFuse C# Unity SDK provides a comprehensive game backend solution through the `GameFuseUser` class, offering complete user management, social features, and game data persistence. Key capabilities include:

**Authentication & User Management**
- User registration, sign-in, password recovery, and profile updates
- Custom user attributes for storing game-specific data
- Score and credits management with server-side validation

**Social Features**
- Friend system with requests, acceptance, and management
- Group creation, membership, and custom group attributes
- Real-time messaging with direct and group chats

**Game Features**
- In-game store with purchasable items and credit transactions
- Leaderboards with score submission and ranking retrieval
- Game rounds tracking for both single-player and multiplayer sessions

**Data Storage**
- Secure cloud storage for user progress and game state
- Custom attributes system for flexible data structures
- Server-side validation preventing client-side manipulation

All features work seamlessly without requiring server setup or API development. GameFuse is free for hobby and small indie projects, with usage-based pricing for larger applications.

## Getting Started

The first step of integrating GameFuse with your project is to make an account and configure your game settings.

[Sign Up](https://gamefuse.co/users/sign_up){ .md-button }

After creating your account, add your first game and note the **Game ID** and **API Token** from your GameFuse dashboard.

### Configuring GameFuse Settings

GameFuse uses a ScriptableObject-based configuration system that requires no prefabs in your scene. After installing the SDK, create your settings file:

1. In Unity, go to **Assets > Create > GameFuse > Settings**
2. Name the file `GameFuseSettings` and place it in a **Resources** folder
3. In the Inspector, enter your **Game ID** and **Game API Key** from your GameFuse dashboard
4. Optionally configure advanced settings like API timeout and retry attempts

The `GameFuseSettings` ScriptableObject will be automatically loaded by the SDK when needed. You can now use GameFuse functionality throughout your project without adding any prefabs to your scenes - simply call the static methods on `GameFuseUser` to sign in users and access all features.

### Installing the SDK

You can easily add GameFuse to your Unity project by editing your project's `manifest.json` file. Add the following line to your dependencies:

```json
"com.gamefuse.sdk": "https://github.com/game-fuse/game-fuse-cs.git?path=com.gamefuse.sdk#v3api"
```

Unity will automatically import the package for you.

[C# Library](https://github.com/game-fuse/game-fuse-cs){ .md-button }

