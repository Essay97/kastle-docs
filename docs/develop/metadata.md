# Metadata

Each game can have some metadata that will be printed at the beginning of the game.
In this section we'll learn something more about them.

The available and configurable metadata are:

- the game name
- the author's name
- the game version
- the publication date
- the Kastle versions that this verision of the game is compatible with

None of these data is mandatory and their purpose is purely documentation (e.g. even if we specify the Kastle versions,
Kastle won't prevent us from running the game in an incompatible version).

The `metadata` block must be put directly into the `game` block. Here's an example:

```kotlin
import kotlinx.datetime.LocalDate

// Inside the game block:
metadata {
    author = "Enrico Saggiorato"
    name = "Tutorial Game"
    version = "1.0.0"
    kastleVersions = listOf("0.1.0") // CLI versions you have tested
    published = LocalDate(2024, 5, 24)
}
```
