# Forgot Password

You can implement this simple method in your app and we will handle all the emails and password reset on our end. Once you hit this function, our system will send an email to that user if they exist, branded like your app. It will have your app's name, image logo and color so it will look cohesive. The sender email is even masked with your apps name. The user will reset their password online and then will be instructed that they can login to your app.

```cpp
void UMyObject::ResetUserPassword()
{
  FManagerCallback CompletionCallback;
  CompletionCallback.BindDynamic(this, &UMyObject::OnMyLeaderboardsClearedCallback);

  UGameFuseManager::SendPasswordResetEmail("example@gmail.com", CompletionCallback);
}

void UMyObject::OnResetUserPasswordCallback(bool bSuccess, const FString& Response)
{
  if(bSuccess)
  {
    UE_LOG(LogTemp, Display, TEXT("Game Connected Successfully"));
    UE_LOG(LogTemp, Display, TEXT("Result : %s"), *Response);
  }else
  {
    UE_LOG(LogTemp, Error, TEXT("Error connecting game"));
    UE_LOG(LogTemp, Error, TEXT("Result : %s"), *Response);
  }
}

```

```
* 404 - Game ID or Token incorrect
* 403 - Invalid Email Address
* 404 - No user found with email
* 500 - unknown server error

```
## User Experience

The user will then receive an email looking like the following:

![password](images/password_reset.png)

When the user clicks on the forgot password link, they will see something like:

![password](images/password_2.png)