# Your first game

We develop our game by implementing the `provideConfiguration` function of the `GameProvider`. Let's use the `game`
function, the entry point of our DSL, to instantiate a new game:

```kotlin
class ExampleGame : GameProvider {
    override fun provideConfiguration(): GameConfiguration = game("r-start") {
        // Your game will go here
    }
}
```

Tha string `"r-start"` that we are passing to the function is the ID of the first room, the one where the player will
start their adventure. Let's give a definition of that room.

## Defining rooms

Let's start defining the initial room:

```kotlin hl_lines="3-7"
class ExampleGame : GameProvider {
    override fun provideConfiguration(): GameConfiguration = game("r-start") {
        // Create a room with "r-start" ID
        room("r-start") {
            name = "Initial room"
            description = "This is the first room that the player sees"
        }
    }
}
```

Then we define another room so that the player can move around:

```kotlin
room("r-next") {
    name = "Another room"
    description = "This is a room that the player can move to"
}
```

Let's link the rooms so that the player can move around. The second room will be to the north of the first:

```kotlin hl_lines="5-8"
room("r-start") {
    name = "Initial room"
    description = "This is the first room that the player sees"
    // Linking the north direction to the "r-next" room
    north("r-next") {
        behavior = LinkBehavior.CONSTANT
        state = LinkState.OPEN
    }
}

room("r-next") {
    name = "Another room"
    description = "This is a room that the player can move to"
}
```

And to make sure that our game can end, let's add a winning condition: when the player reaches the `"r-next"` room,
the game ends. Let's add this block under the second room definition:

```kotlin
winIf {
    playerEnters = "r-next"
}
```

## Defining the player

You can rest assured that every adventure has its hero! We'll define ours with the following lines:

```kotlin
player {
    name = "Enrico"
    description = "Enrico is the author of Kastle and also an awesome hero for this game"
}
```

I'm giving my name to the hero, but feel free to put yours!

## Building the game

This little example might be not that fun to play, but it really is what it takes to create a fully functional Kastle game,
so let's build it!

At the end of the day, a Kastle game is really just a Gradle project, so we build it by running `./gradlew build` (`gradlew.bat build` on Windows). If the command is executed without errors, then you'll find the jar file containing the game under `build/libs/<project name>-<version>.jar`.

## Complete example

With the project name and version from [Project setup](project-setup.md), use this complete `ExampleGame.kt`:

```kotlin
package com.example.mypackage

import com.saggiodev.kastle.dsl.game
import com.saggiodev.kastle.dto.GameConfiguration
import com.saggiodev.kastle.dto.LinkState
import com.saggiodev.kastle.model.LinkBehavior
import com.saggiodev.kastle.service.GameProvider

class ExampleGame : GameProvider {
    override fun provideConfiguration(): GameConfiguration = game("r-start") {
        room("r-start") {
            name = "Initial room"
            description = "This is the first room that the player sees"
            north("r-next") {
                behavior = LinkBehavior.CONSTANT
                state = LinkState.OPEN
            }
        }
        room("r-next") {
            name = "Another room"
            description = "This is a room that the player can move to"
        }
        player {
            name = "Enrico"
            description = "The hero of this adventure"
        }
        winIf { playerEnters = "r-next" }
    }
}
```

Keep the service descriptor from Project setup. Build, install and play with:

```sh
./gradlew build
kastle install com.example.mypackage.ExampleGame build/libs/tutorial-game-1.0.0.jar --name tutorial
kastle play tutorial
```

Use the CLI built from the same monorepo checkout as the API when testing current-source changes. At the in-game prompt, `inspect room` describes the room and `go north` reaches the winning room.

For another working game, see the monorepo's [sample-game](https://github.com/Essay97/kastle-monorepo/tree/main/sample-game).
