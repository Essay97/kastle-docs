# Installing games

A Kastle game is a JAR file. Obtain a compatible game from its author along with the fully qualified name of its `GameProvider` class. Kastle does not download games from a built-in catalogue.

After [installing the engine](installation.md), run:

```sh
kastle install com.example.AdventureGame ./adventure.jar --name adventure
kastle play adventure
```

Replace the provider class and JAR path with those supplied by the author. The class comes first, followed by the file path. `--name` (or `-n`) chooses the installed name used by `play`, `info`, and `uninstall`. Without it, the name defaults to the JAR filename without its extension. Quote paths or names containing spaces in your shell.

Installation checks the JAR's provider registrations and constructs its providers. Game-definition validation happens when you start the game. Only install games from sources you trust: provider constructors execute JVM code during installation.

An installed name, provider class, or JAR filename cannot be reused by another installation. To replace a game, uninstall its existing entry first; an install never silently overwrites it.

For a game you can build yourself, follow the [developer quickstart](../develop/project-setup.md). Continue with the [Player Guide](../play.md) for managing games and in-game commands.
