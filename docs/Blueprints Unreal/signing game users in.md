# Signing Game Users In

GameFuse provides a secure authentication system that allows registered users to sign in to your game from any device.

## Overview

The sign-in process authenticates users using their email and password. Once successfully signed in, users can access their saved data, credits, store items, and other GameFuse features.

## User Authentication

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/x2ult6su/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

    **Important:** Email and password (not username) are required for sign-in.

## User Logout

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/_8l3payu/" width="800" height="600" frameborder="0" allowfullscreen></iframe>


## Function Parameters

### Sign In

| Parameter | Type | Description |
|-----------|------|-------------|
| `Email` | `String` | User's registered email address |
| `Password` | `String` | User's password |

### Log Out

No parameters required - simply clears the current user session.

## Function Return Values

### Sign In

| HTTP Status Code | Description |
|------------------|-------------|
| `200` | OK - User signed in successfully |
| `400` | Bad request - Invalid parameters |
| `401` | Unauthorized - Invalid email or password |
| `500` | Unknown server error |

### Log Out

| HTTP Status Code | Description |
|------------------|-------------|
| `200` | OK - User logged out successfully |
| `500` | Unknown server error |

## Session Management

After successful sign-in:
- User data is automatically cached locally
- All GameFuse features become available
- User remains signed in until logout or session expiry

## Security Considerations

- Never store passwords in plain text
- Use secure methods for credential storage
- Implement proper session timeout handling
- Consider implementing multi-factor authentication for sensitive games

## Next Steps

After successful sign-in:
- [Using Credits](using%20credits.md) - Learn about the credit system
- [Using the Store](using%20the%20store%20in%20your%20game.md) - Access store functionality
- [Custom User Data](custom%20user%20data.md) - Work with user-specific data
- [Friends](friends.md) - Implement social features
