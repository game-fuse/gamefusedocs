# Signing Game Users Up

GameFuse provides a comprehensive user registration system that allows players to create accounts, which are saved online and accessible across devices.

## Overview

Enable users to sign up in your Unreal Engine game using GameFuse's user management system. These users will be saved in your GameFuse game database and can then login from other devices, since the data is saved online.

## User Registration

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/rpej2u3g/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

## Function Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `Email` | `String` | User's email address (must be unique and valid format) |  
| `Password` | `String` | User's password (minimum requirements set in GameFuse dashboard) |
| `Password Confirmation` | `String` | Must exactly match the password field |
| `Username` | `String` | User's display name (must be unique) |

## Function Return Values

| HTTP Status Code | Description |
|------------------|-------------|
| `200` | OK - User registered successfully |
| `400` | Bad request - Invalid parameters or validation errors |
| `409` | Conflict - Email or username already exists |
| `500` | Unknown server error |

## Next Steps

After successful registration:
- [Sign In Users](signing%20game%20users%20in.md) - Learn how to authenticate users
- [Using Credits](using%20credits.md) - Learn about the credit system
- [Custom User Data](custom%20user%20data.md) - Learn how to store additional user information
