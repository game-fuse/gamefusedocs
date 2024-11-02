You can implement this simple method in your app and we will handle all the
emails and password resets on our end.

Once you run this function, our system will send an email to that user if it
exists. The email will be branded like your app: it will have your app's name,
image logo and color so it will look cohesive. The sender's email is even
masked with your app's name.

The user will then reset their password online and then will be instructed that
they can login into your app.

## Password reset

### Scope

Resets a user's password.

### Method

!!! info annotate "GET"
    ```plaintext
    /api/v3/games/{gameId}/forget_password?email={email}&game_id={gameId}&game_token={gameToken}
    ```

### Attributes

| Name             | Type          | Required | Description |
|------------------|---------------|----------|-------------|
| `gameId`         | integer       | Yes      | Found on your GameFuse.co dashboard |
| `gameToken`      | string        | Yes      | API token found on your GameFuse.co dashboard | 
| `email`          | string        | Yes      | Email address of user to send the forgot password form to |

### Data (payload)

None

### Headers

| Name | Type | Description |
|----------|---------|--------------|
| `authentication-token` | string | Found in sign-in or sign-up responses. This token is used for user sessions |

### Responses

| HTTP status code | content-type | Description |
|------------------|--------------|-------------|
| `200`            | application/json         | Object containing the response and a message |
| `403`            | text/plain | Invalid Email Address |
| `404`            | text/plain | No user found with specified `email`, or `gameId` or `gameToken` incorrect |
| `500`            | text/plain | Unknown server error |

### Response object

| Attribute name                    | Type | Description |
|-----------------------------------|------|-------------|
| `mailer_response`                 | string | Details of the emailer |
| `message`                         | string | Message confirming email was sent or with information that the user does not exists |

### Examples

!!! example
    ### cURL

    ```shell
    curl --request GET \
        --header "authentication-token: abc123" \
        "https://gamefuse.co/api/v3/games/1/forget_password?email=john.doe@example.com&game_id=1&game_token=abc123"
    ```

    ### Response

    ```json
    {
        "mailer_response": null,
        "message": "Email sent to john.doe@example.com"
    }
    ```

### User Experience

See <a href="/generic/forgot_password/#user-experience">Generic: Forgot password - User experience</a>.
