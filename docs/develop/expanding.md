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
