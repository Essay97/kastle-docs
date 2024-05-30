# Starting and ending the game

In this section we'll see how to introduce the player to the game world and how to set victory requirements.

## Introducing the game

Kastle allows a preface: a sentence or small text that introduces the game. You can use it to describe the backstory, give some context about the world in which the game is set and so on.

You can do that with the `preface` keyword right into the `game` block:

```kotlin
game {
    preface = """
        This is the preface of this game.
        As you can see here, by exploiting Kotlin multiline strings, we can provide a quite long text as a preface,
        so in a real game we would have plenty of space to contextualize our game!
    """.trimIndent
}
```

## Setting winning conditions

A Kastle game can end because of two victory conditions:

- the player has a specific object in the inventory
- the player is in a specific room
  It is sufficient that the player fulfills one of the defined requirements and the game ends.

We can define such requirements thanks to the `winIf` block inside the `game` block:

```kotlin
winIf {
    playerEnters = "r-next"
    playerOwns = "i-medal" // (1)!
}
```

1. This item hasn't been defined yet, but don't worry, we'll address it later!

Notice that the winner room and the winner item are referenced by their ID and that we are not forced to use both.
