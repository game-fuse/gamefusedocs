# GameFuse Settings Configuration

The `GameFuseSettings` ScriptableObject is the central configuration system for the GameFuse Unity SDK. It provides a clean, inspector-friendly way to manage your game's connection settings and SDK behavior without requiring any prefabs in your scenes.

## Overview

`GameFuseSettings` is a ScriptableObject that stores all necessary configuration for your GameFuse integration:

- **Game ID and API Key** - Your unique game credentials from the GameFuse dashboard
- **API Configuration** - Base URL, timeout settings, and retry behavior
- **Automatic Loading** - The SDK automatically finds and loads your settings at runtime

## Creating GameFuse Settings

### Step 1: Create the Settings Asset

1. In Unity, navigate to **Assets > Create > GameFuse > Settings**
2. Name the file `GameFuseSettings` (this name is important for automatic discovery)
3. Place the file in a **Resources** folder in your project
   - If you don't have a Resources folder, create one: **Assets > Create > Folder** and name it "Resources"

### Step 2: Configure Your Settings

Select your `GameFuseSettings` asset and configure the following in the Inspector:

#### Required Settings

| Field | Description | Example |
|-------|-------------|---------|
| **Game Id** | Your unique game identifier from the GameFuse dashboard | `"1234"` |
| **Game Api Key** | Your game's API key from the GameFuse dashboard | `"abcd1234efgh5678"` |

#### Optional Advanced Settings

| Field | Description | Default | Range |
|-------|-------------|---------|-------|
| **Api Base Url** | GameFuse API endpoint (only change if using custom endpoint) | `"https://gamefuse.co/api/v3"` | - |
| **Max Retry Attempts** | Number of retry attempts for failed API requests | `3` | 0-5 |
| **Request Timeout Seconds** | Timeout for API requests in seconds | `30` | 5-60 |

## How It Works

### Automatic Discovery

The SDK uses Unity's `Resources.Load<>()` system to automatically find your settings:

```csharp
// The SDK automatically loads settings like this:
var settings = Resources.Load<GameFuseSettings>("GameFuseSettings");
```

### Static Access

Once loaded, settings are available throughout your project via the static property:

```csharp
using GameFuse.Config;

// Access settings anywhere in your code
var gameId = GameFuseSettings.Settings?.GameId;
var apiKey = GameFuseSettings.Settings?.GameApiKey;
```

### Integration with GameFuseUser

The `GameFuseUser` class automatically uses these settings for authentication:

```csharp
// No need to pass Game ID or API Key - they're read from settings
var user = await GameFuseUser.SignInAsync("username", "password");

// You can still override settings if needed
var user2 = await GameFuseUser.SignInAsync("username", "password", "customGameId", "customApiKey");
```

## Usage Examples

### Basic Setup

```csharp
using GameFuse;
using UnityEngine;

public class GameManager : MonoBehaviour
{
    async void Start()
    {
        try
        {
            // GameFuseSettings is automatically loaded and used
            var user = await GameFuseUser.SignInAsync("player@example.com", "password123");
            Debug.Log($"Signed in as: {user.Username}");
        }
        catch (Exception e)
        {
            Debug.LogError($"Sign in failed: {e.Message}");
        }
    }
}
```

### Checking Settings Availability

```csharp
using GameFuse.Config;
using UnityEngine;

public class SettingsChecker : MonoBehaviour
{
    void Start()
    {
        var settings = GameFuseSettings.Settings;
        
        if (settings == null)
        {
            Debug.LogError("GameFuseSettings not found! Create one via Assets > Create > GameFuse > Settings");
            return;
        }
        
        if (string.IsNullOrEmpty(settings.GameId) || string.IsNullOrEmpty(settings.GameApiKey))
        {
            Debug.LogError("GameFuseSettings found but Game ID or API Key is missing!");
            return;
        }
        
        Debug.Log("GameFuse settings configured correctly!");
    }
}
```

### Runtime Settings Override

```csharp
using GameFuse;
using GameFuse.Config;

// You can still override settings at runtime if needed
public async void SignInWithCustomCredentials()
{
    // This will use custom credentials instead of GameFuseSettings
    var user = await GameFuseUser.SignInAsync(
        "username", 
        "password", 
        gameId: "alternativeGameId", 
        gameApiKey: "alternativeApiKey"
    );
}
```

## Error Handling

### Missing Settings File

If no `GameFuseSettings` file is found in Resources, you'll see this error:

```
GameFuseSettings ScriptableObject not found in a Resources folder. 
Please create one via Assets > Create > GameFuse > Settings and populate GameId and GameApiKey.
```

**Solution**: Create the settings file as described above.

### Missing Credentials

If Game ID or API Key are not set, authentication methods will throw:

```
ArgumentNullException: Game ID must be provided explicitly or through GameFuseSettings.
```

**Solution**: Set the Game ID and API Key in your GameFuseSettings asset.

## Best Practices

### File Organization

```
Assets/
├── Resources/
│   └── GameFuseSettings.asset    # Must be in Resources folder
├── Scripts/
│   └── GameManager.cs
└── Scenes/
    └── MainMenu.unity
```

### Security Considerations

- **Never commit API keys to public repositories**
- Consider using different settings files for development/production
- Use Unity's build configurations to swap settings files automatically

### Multiple Environments

For different environments (development, staging, production), you can:

1. Create multiple settings files (e.g., `GameFuseSettings_Dev`, `GameFuseSettings_Prod`)
2. Use build scripts to copy the appropriate file to `GameFuseSettings` before building
3. Or override settings at runtime based on build configuration

## Troubleshooting

### Settings Not Loading

**Problem**: `GameFuseSettings.Settings` returns `null`

**Solutions**:
- Ensure the file is named exactly `GameFuseSettings`
- Ensure the file is in a folder named `Resources`
- Check the Console for error messages about missing settings

### Authentication Fails

**Problem**: Sign-in/sign-up methods throw authentication errors

**Solutions**:
- Verify Game ID and API Key are correct in your GameFuse dashboard
- Check that the values are properly set in your GameFuseSettings asset
- Ensure you're using the correct API base URL (default should work for most cases)

### API Timeouts

**Problem**: Requests are timing out or failing

**Solutions**:
- Increase `Request Timeout Seconds` in your settings
- Increase `Max Retry Attempts` for unstable connections
- Check your internet connection and firewall settings