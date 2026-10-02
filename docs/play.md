# Player guide

Kastle runs text adventures supplied as game JAR files. Install the [CLI](getting-started/installation.md), obtain a compatible game from its author, then install and launch it as described below.

This guide describes the current monorepo implementation. Older released CLI builds may differ, particularly in how item and character names are recognized.

## Manage games from your terminal

These commands run in your operating system's terminal, outside a game. Use `kastle --help` or a command's help, such as `kastle install --help`, to see its arguments.

### Install a game

The author must supply the JAR and the fully qualified name of its game provider class. For example, for a JAR called `adventure.jar` containing `com.example.AdventureGame`:

```sh
kastle install com.example.AdventureGame ./adventure.jar --name adventure
```

Replace the class and path with those supplied by the author. The class comes **before** the JAR path. `-n adventure` is equivalent to `--name adventure`. Quote paths or installed names containing spaces in your shell.

The name is the local label used by `info`, `play`, and `uninstall`; it need not match the title displayed inside the game. If you omit `--name`, Kastle uses the JAR filename without its extension. Currently, the success message can say `null installed correctly.` when no name is supplied; use `kastle list` to check the installed name.

Kastle copies the JAR into `~/.kastle/games` and records the installation in `~/.kastle/games.db` (under the Java user's home directory). Installation does not validate that the JAR can actually be played. Use a compatible JAR from a source you trust: games execute JVM code on your computer.

There is no upgrade command. To replace an installed game, uninstall its existing entry before installing the replacement. Installation and removal error handling is still limited; an invalid JAR or missing game can produce an exception rather than a friendly error. A failed installation may leave an entry behind, so check `list` and `info` before retrying.

### List installed games

```sh
kastle list
kastle ls
```

Both commands list installed names, one per line, or print `No games found`. They do not display JAR paths.

### Inspect an installation

```sh
kastle info adventure
```

This prints the installed name, `Game file:` (the JAR filename), and `Main class:` (the provider class). It does not display the game's story, author, or version metadata.

### Start a game

```sh
kastle play adventure
```

Use the installed name from `kastle list`. Kastle loads the game, displays its header and preface, then asks `What do you want to do?`. Enter the commands below at the `>` prompt and press **Enter**.

Each launch starts a new playthrough. There is currently no save or load command.

### Uninstall a game

After leaving the game:

```sh
kastle uninstall adventure
```

This removes the installation record and Kastle's copied JAR. It leaves the original JAR you supplied to `install` in place. Use an existing installed name from `kastle list`.

## Enter in-game commands

Write command words and directions in lowercase. Use the full directions `north`, `south`, `east`, and `west`; shortcuts such as `n` are not supported. There is no in-game `help` command; use this guide as the command reference.

Item and character targets accept their complete name or an alias supplied by the author, ignoring case and surrounding whitespace. For an item named **Brass Key**, enter `inspect brass key` or `inspect BRASS KEY`. Enter multiword names directly, **without quotes**. `inspect key` works only if the author also supplied `key` as an alias. Partial matching is not supported, and spaces inside a name remain significant.

Names and available actions depend on the game. The examples below assume a room containing a **Brass Key** and a character named **Guide**, with a passage to the north; substitute the names and directions in your adventure.

| Command | What it does |
| --- | --- |
| `where` | Show the current room's name and description. |
| `who` | Show your player character's name and description. |
| `inspect room` | Show the current room's description. |
| `inspect brass key` | Inspect a matching item in the room or your inventory. |
| `inspect guide` | Inspect a matching character in the current room. |
| `inventory` | List the items you carry, or report that the inventory is empty. |
| `grab brass key` | Move a portable item from the current room into your inventory. |
| `drop brass key` | Move an item from your inventory into the current room. |
| `go north` | Traverse an open passage in that direction. |
| `open north` | Try to open the passage in that direction. |
| `close north` | Try to close the passage in that direction. |
| `talk guide` | Start a conversation with a character in the current room. |
| `end` | Exit the playthrough immediately, without saving. |

`where`, `who`, `inventory`, and `end` take no arguments. The other commands require a target or direction. A failed ordinary game action reports an error and lets you try another command.

### Explore and inspect

Start with `where` to read your surroundings and `who` to learn about your character. Movement also displays the destination room's name and description unless that move wins the game.

Room descriptions are written by the author; they are not automatically updated lists of exits, characters, or loose items. `inspect room` repeats the description without the room-name heading. Inspecting an item or character only shows its description; it does not collect an item or start a conversation.

A passage must exist and be open before you can use `go`. Connections are directional: reaching a room does not guarantee that the opposite direction leads back.

### Carry and use items

Only items marked as portable by the author can be grabbed. For example:

```text
grab brass key
inventory
inspect brass key
drop brass key
grab brass key
```

Dropping an item leaves it in your current room, where it can be picked up again. You cannot grab an item from another room or drop something you do not carry.

There is no separate `use` command. To operate a passage, carry one of the items the author designated as its key or requirement, then enter a direction:

```text
open north
close north
open north
go north
```

This sequence only works for a passage that permits both opening and closing. Some passages are fixed, open-only, or close-only. Both `open` and `close` require a matching item in your inventory; having one of several accepted items is sufficient. The item is not consumed. Operating a passage affects that direction from the current room, not automatically the reverse connection.

Currently, successful open/close messages may repeat the source room's name instead of naming the destination. The action still applies to the direction you entered.

### Talk and choose answers

```text
talk guide
```

When answers appear, use **Up** and **Down** to select one, then **Enter** to confirm. The displayed numbers are labels, not keys to type. Some conversations consist only of a final line and require no choice.

A character's conversation can be started only once per playthrough. Your choices can lead to different endings. If an ending awards an item, it appears in the current room, not directly in your inventory. For example, after a guide offers a portable **Coin**:

```text
inspect coin
grab coin
inventory
```

Only the ending you reach provides its reward. Repeating `talk guide` does not restart the conversation or grant another reward.

### Finish a playthrough

The game author defines victory conditions. When you satisfy them, Kastle displays the ending and `You won!`, then closes the game automatically.

To leave before winning, enter:

```text
end
```

There is no confirmation or saved progress. Running `kastle play adventure` again starts over with the game's initial rooms, items, and conversations.
