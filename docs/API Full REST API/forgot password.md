You can implement this simple method in your app and we will handle all the emails and password reset on our end. Once you hit this function, our system will send an email to that user if they exist, branded like your app. It will have your app's name, image logo and color so it will look cohesive. The sender email is even masked with your apps name. The user will reset their password online and then will be instructed that they can login to your app.

```
https://gamefuse.co/api/v1/games/{game_id}/forget_password?email={email}&game_id={game_id}&game_token={game_token}

```

It will have the following URL parameters:

```
game_id: {found on your GameFuse.co dashboard}
game_token: {API token found on your GameFuse.co dashboard}
email: {email address of user to send forgot password form to}

```

This will return a response containing all the users attributes (custom data) with the following json:

```
{
    "mailer_response": {detail of the emailer},
    "message": {message confirming email sent or with info that user does not exist}
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