# Forgot Password

You can implement this simple method in your app and we will handle all the
emails and password resets on our end.

Once you run this function, our system will send an email to that user if it
exists. The email will be branded like your app: it will have your app's name,
image logo and color so it will look cohesive. The sender's email is even
masked with your app's name.

The user will then reset their password online and then will be instructed that
they can login into your app.

!!! example
    ```jsx
    Start(){
        let self = this;
        GameFuse.sendPasswordResetEmail("example@gmail.com, function (message, hasError)
        {
            if (hasError)
            {
                alert("Something went wrong...");
            }
            else
            {
                alert("A link to reset your password has been sent to your email!");
            }
        });
    }
    ```

## Function return values

### `GameFuse.SendPasswordResetEmail`

| HTTP status code | Description |
|------------------|-------------|
| `200`            | OK |
| `403`            | Invalid email address |
| `404`            | No user found with the specified email, or GameID or Token incorrect |
| `500`            | Unknown server error |

## User Experience

See [Generic: Forgot password - User experience](../generic/forgot_password.md/#user-experience).
