# Using Credits

Credits are GameFuse's virtual currency system. Each user has a credit balance that can be used to purchase store items, unlock features, or power your game's economy.

## Overview

Credits are a numeric attribute of each game user stored as a simple integer value. They can be added manually through API calls and are automatically deducted when users purchase store items.

## Getting User Credits

You can retrieve the current user's credit balance:

!!! example
    ```cpp
    void UMyObject::GetUserCredits()
    {
        // Get the GameFuse User subsystem
        UGameFuseUser* GameFuseUser = GetGameInstance()->GetSubsystem<UGameFuseUser>();
        
        // Get current user data
        const FGFUserData& UserData = GameFuseUser->GetCurrentUserData();
        UE_LOG(LogTemp, Display, TEXT("Current credits: %d"), UserData.Credits);
    }
    ```

## Adding Credits

You can add credits to the current user's account:

!!! example
    ```cpp
    void UMyObject::AddCredits(int32 CreditsToAdd)
    {
        // Get the GameFuse User subsystem
        UGameFuseUser* GameFuseUser = GetGameInstance()->GetSubsystem<UGameFuseUser>();
        
        // Create a typed callback for better type safety
        FGFUserDataCallback CompletionCallback;
        CompletionCallback.BindLambda([this](bool bSuccess, const FGFUserData& UserData)
        {
            if(bSuccess)
            {
                UE_LOG(LogTemp, Display, TEXT("Credits added successfully"));
                UE_LOG(LogTemp, Display, TEXT("New credit balance: %d"), UserData.Credits);
            }
            else
            {
                UE_LOG(LogTemp, Error, TEXT("Failed to add credits"));
            }
        });
        
        // Add credits to the user
        GameFuseUser->AddCredits(CreditsToAdd, CompletionCallback);
    }
    ```

## Setting Credits

You can set the user's credits to a specific amount:

!!! example
    ```cpp
    void UMyObject::SetCredits(int32 NewCreditAmount)
    {
        // Get the GameFuse User subsystem
        UGameFuseUser* GameFuseUser = GetGameInstance()->GetSubsystem<UGameFuseUser>();
        
        // Create a typed callback for better type safety
        FGFUserDataCallback CompletionCallback;
        CompletionCallback.BindLambda([this](bool bSuccess, const FGFUserData& UserData)
        {
            if(bSuccess)
            {
                UE_LOG(LogTemp, Display, TEXT("Credits set successfully"));
                UE_LOG(LogTemp, Display, TEXT("New credit balance: %d"), UserData.Credits);
            }
            else
            {
                UE_LOG(LogTemp, Error, TEXT("Failed to set credits"));
            }
        });
        
        // Set user's credits to specific amount
        GameFuseUser->SetCredits(NewCreditAmount, CompletionCallback);
    }
    ```

## Function Parameters

### Add Credits

| Parameter | Type | Description |
|-----------|------|-------------|
| `Credits` | `int32` | The amount of credits to add (must be positive) |
| `Callback` | `FGFUserDataCallback` | Callback function to handle the response |

### Set Credits

| Parameter | Type | Description |
|-----------|------|-------------|
| `Credits` | `int32` | The target credit amount to set (must be non-negative) |
| `Callback` | `FGFUserDataCallback` | Callback function to handle the response |

## Function Return Values

### Add/Set Credits

| HTTP Status Code | Description |
|------------------|-------------|
| `200` | OK - Credits updated successfully |
| `400` | Bad request - Invalid parameters (negative values, etc.) |
| `401` | Unauthorized - User not signed in |
| `500` | Unknown server error |
