# Expanding the game

The game that we created in the previous section is just the bare minimum for Kastle to work. Let's build upon that and explore some of the other features of Kastle.

## Adding items

A room can contain items. Let's put a table in the initial room:

```kotlin hl_lines="5-8"
room("r-start") {
    name = "Initial room"
    description = "This is the first room that the player sees"

    item("i-table") {
        name = "Table"
        description = "A nice wooden table"
    }
}
```

## Adding NPCs

Also NPCs can be found in a room. Let's add one right under the `item` block:

```kotlin
character("c-doorman") {
    name = "Jack"
    description = "Jack is the official Kastle's doorman!"
}
```

### Adding dialogue

An NPC like this is not that much useful, right? We want the characters of our stories to be dynamic and interactive. The following block adds a simple dialogue to the doorsman. You can add it inside the `character` block:

```kotlin
character("c-doorman") {
    name = "Jack"
    description = "Jack is the official Kastle's doorman!"
}
```
