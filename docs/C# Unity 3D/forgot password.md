# Forgot Password

You can implement this simple method in your app and we will handle all the emails and password reset on our end. Once you hit this function, our system will send an email to that user if they exist, branded like your app. It will have your app's name, image logo and color so it will look cohesive. The sender email is even masked with your apps name. The user will reset their password online and then will be instructed that they can login to your app.

```csharp
void Start(){
    GameFuseUser.Instance.SendPasswordResetEmail("example@gmail.com", ForgotPasswordEmailSent);
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

```
* 404 - Game ID or Token incorrect
* 403 - Invalid Email Address
* 404 - No user found with email
* 500 - unknown server error
```