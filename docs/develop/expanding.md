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

An NPC like this is not that much useful, right? We want the characters of our stories to be dynamic and interactive.
The following block adds a simple dialogue to the doorsman. You can add it inside the `character` block:

```kotlin hl_lines="4-34"
character("c-doorman") {
    name = "Jack"
    description = "Jack is the official Kastle's doorman!"
    dialogue {
        firstQuestion("d-first") { // (1)!
            text = "Hey player! are you ready for the adventure?"
            answer {
                text = "Yes!"
                nextQuestion = "d-ifyes" // (2)!
            }
            answer {
                text = "No..."
                nextQuestion = "d-ifno"
            }
        }

        question("d-ifyes") {
            text = "Ok, take this and go the the next room!"
            reward("i-diploma") { // (3)!
                name = "Diploma"
                description = """
                    It's a nice diploma with decorated borders. It says:
                    'Well done, player!

                    You reached the end of your first Kastle game.
                    Move to the north to win the game!'
                """.trimIndent() // (4)!
            }
        }

        question("d-ifno") {
            text = "No problem, I'll wait until you're ready, but you're missing a lot of fun!"
        }
    }
}
```

1. The `firstQuestion` block is mandatory to let Kastle know where to start
2. Here we are defining the ID of the next question. Make sure to define a question with this ID later on!
3. If the player reaches this branch of the dialogue, they get rewarded with an item. Inside the `reward` block,
   you can define an item with the exact same DSL that we used before.
4. Remember that the DSL is first and foremost Kotlin code! We can leverage all the features of the language,
   such as multiline strings and the `trimIndent` function in this case.

The Dialogue DSL is for sure the the most complex in Kastle, but it really gives depth to your characters and your story!
