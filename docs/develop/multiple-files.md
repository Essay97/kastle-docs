# Splitting the game across multiple files

As your game grows, maintaining and expanding it can become challenging. With the Kastle DSL, you can easily split the game across multiple files that come together in your GameProvider class.

It's highly recommended to start by developing the game within the provideConfiguration function, and then, as needed, refactor parts into separate files. A valuable example is the Dialogue DSL: it usually takes up much space and could lead to losing the general idea of the game configuration. Let's get back to our example and extract the dialogue into another file. We start from this snippet:

```kotlin
character("c-doorman") {
    name = "Jack"
    description = "Jack is the official Kastle's doorman!"

    dialogue {
        firstQuestion("d-first") {
            text = "Hey player! are you ready for the adventure?"
            answer {
                text = "Yes!"
                nextQuestion = "d-ifyes"
            }
            answer {
                text = "No..."
                nextQuestion = "d-ifno"
            }
        }

        question("d-ifyes") {
            text = "Ok, take this and go the the next room!"
            reward("i-diploma") {
                name = "Diploma"
                description = """
        It's a nice diploma with decorated borders. It says:
        'Well done, player!

        You reached the end of your first Kastle game.
        Move to the north to win the game!'
    """.trimIndent()
            }
        }

        question("d-ifno") {
            text = "No problem, I'll wait until you're ready, but you're missing a lot of fun!"
        }
    }
}
```

The `dialogue` function is in the scope of the `character` block. Let's take a look at the definition of the `character` function:

```kotlin
fun character(characterId: String, init: CharacterScope.() -> Unit)
```

Its `init` block has a `CharacterScope` receiver, so if we want to extract our dialogue into another file, we just have to define an extension function for the `CharacterScope`, for example:

```kotlin
import io.github.essay97.kastle.dsl.CharacterScope

fun CharacterScope.doormanDialogue() {
    dialogue {
        firstQuestion("d-first") {
            text = "Hey player! are you ready for the adventure?"
            answer {
                text = "Yes!"
                nextQuestion = "d-ifyes"
            }
            answer {
                text = "No..."
                nextQuestion = "d-ifno"
            }
        }

        question("d-ifyes") {
            text = "Ok, take this and go the the next room!"
            reward("i-diploma") {
                name = "Diploma"
                description = """
                    It's a nice diploma with decorated borders. It says:
                    'Well done, player!

                    You reached the end of your first Kastle game.
                    Move to the north to win the game!'
                """.trimIndent()
            }
        }

        question("d-ifno") {
            text = "No problem, I'll wait until you're ready, but you're missing a lot of fun!"
        }
    }
}
```

And then our character would become:

```kotlin
character("c-doorman") {
    name = "Jack"
    description = "Jack is the official Kastle's doorman!"

    doormanDialogue()
}
```

Much more readable! We could go further and extract each question into its own function but the functioning is always the same: look at the API docs for the receiver type and define an extension function.

## Reusing code

Extracting some parts of the DSL into separate functions can enhance code reuse. As an example, imagine that we want to place a guard in each room of our game. Every guard has the same behavior, but each one has their own name. We colud define a function like the following:

```kotlin
fun RoomScope.guard(id: String, name: String) {
    character(id) {
        this.name = name
        description = "This guard won't let you pass"
    }
}
```
