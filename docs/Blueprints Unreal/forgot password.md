# Forgot Password

The GameFuse Forgot Password system provides a seamless way to help users recover their accounts when they forget their login credentials.

## Overview

You can implement this simple method in your app and GameFuse will handle all the emails and password resets on the backend. The system automatically sends branded emails that match your app's appearance.

## Password Recovery Process

!!! example "Blueprint Example"
    <iframe src="https://blueprintue.com/render/2fyyxgwn/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

## Function Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `Email` | `String` | The email address of the user requesting password reset |

## Function Return Values

| HTTP Status Code | Description |
|------------------|-------------|
| `200` | OK - Password reset email sent successfully |
| `400` | Bad request - Invalid email format |
| `404` | Email address not found in system |
| `500` | Unknown server error |

## How It Works

1. **User Requests Reset**: Player enters their email in your app
2. **Email Sent**: GameFuse sends a branded password reset email
3. **User Clicks Link**: Email contains a secure reset link
4. **Password Reset**: User creates a new password on GameFuse website
5. **Return to App**: User is instructed to login with new password

## Email Branding

The password reset email is automatically branded with:
- Your app's name
- Your app's logo image
- Your app's color scheme
- Sender email masked with your app's name

This ensures a cohesive user experience that matches your game's branding.


For detailed information about the user experience flow, see [Generic: Forgot Password - User Experience](../generic/forgot_password.md#user-experience).

## Next Steps

- [Signing Game Users In](signing%20game%20users%20in.md) - After password reset
- [Signing Game Users Up](signing%20game%20users%20up.md) - For new users
- [Getting Started](getting%20started.md) - Initial setup guide
