# Rounds System

!!! note
    These Docs are WIP and will be updated with Visual Blueprint Examples. Please see [Class Methods](class%20methods.md) for visual examples.

The GameFuse Rounds System allows you to track and manage game rounds in your game. This includes creating rounds, fetching round data, and managing round metadata.

## Getting Started with Rounds

To use the GameFuse Rounds system in Blueprints, you'll need to access the GameFuse Rounds subsystem:

<!-- <iframe src="https://blueprintue.com/render/your-blueprint-id/" width="800" height="600" frameborder="0" allowfullscreen></iframe>

[Copy Code](https://blueprintue.com/blueprint/your-blueprint-id/) -->

## Creating a Game Round

To create a game round:

1. Get the GameFuse Rounds subsystem
2. Create a new Game Round structure
3. Set properties like Score, Start Time, End Time, and Game Type
4. Call CreateGameRound with the round data
5. Create a callback function to handle the response
6. Process the created round data

## Creating a Game Round with Metadata

To create a game round with metadata:

1. Get the GameFuse Rounds subsystem
2. Create a new Game Round structure
3. Set basic properties
4. Add metadata key-value pairs to the round data
5. Call CreateGameRound with the round data
6. Create a callback function to handle the response
7. Process the created round data and metadata

## Creating a Multiplayer Game Round

To create a multiplayer game round:

1. Get the GameFuse Rounds subsystem
2. Create a new Game Round structure
3. Set the Game Type to "Multiplayer"
4. Add metadata relevant to multiplayer
5. Call CreateGameRound with the round data
6. Create a callback function to handle the response
7. Process the created round data for multiplayer setup

## Fetching a Game Round

To fetch a specific game round:

1. Get the GameFuse Rounds subsystem
2. Call FetchGameRound with the round ID
3. Create a callback function to handle the response
4. Process the round data and metadata

## Fetching User Game Rounds

To fetch all game rounds for the current user:

1. Get the GameFuse Rounds subsystem
2. Call FetchUserGameRounds with page number (e.g., 1)
3. Create a callback function to handle the response
4. Process the array of rounds for display or statistics

## Updating a Game Round

To update an existing game round:

1. Get the GameFuse Rounds subsystem
2. First fetch the existing round using FetchGameRound
3. In the callback, modify the round data (score, end time, metadata)
4. Call UpdateGameRound with the updated round data
5. Create a callback function to handle the update response
6. Process the updated round result

## Deleting a Game Round

To delete a game round:

1. Get the GameFuse Rounds subsystem
2. Call DeleteGameRound with the round ID
3. Create a callback function to handle the response
4. Process the success or failure of the deletion 