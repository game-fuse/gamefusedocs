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
    ```csharp
    void Start(){
        GameFuseUser.Instance.SendPasswordResetEmail("john.doe@example.com", ForgotPasswordEmailSent);
    }

    void ForgotPasswordEmailSent(string message, bool hasError) {
      if (hasError)
      {
          Debug.Log("Error setting attribute: "+message);
      }
      else
      {
          Debug.Log(GameFuseUser.CurrentUser.GetAttributeValue("CURRENT_LEVEL")); // Prints "5"
      }
    }
    ```

## Responses

| HTTP status code | Description |
|------------------|-------------|
| `200`            | Object containing the response and a message |
| `403`            | Invalid email address |
| `404`            | No user found with the specified email, or GameID or Token incorrect |
| `500`            | Unknown server error |
