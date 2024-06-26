# Characters

Characters' main purpose is to talk with the player. Dialogue is not mandatory, for example you may want to model
an impassive guard who never says a word, but as already stated, Dialoge DSL is the most complex of them all, so we'll break it down in this section.

## General character properties

If we don't take into account the dialogue, the Character DSL is very simple: just a name and a description, as we already saw in the previous examples.

Furthermore, just like with items, it's possible to use the `matchers` functions to define alternative names that refer to the character.
!!! info "Character ID"

    The ID of any character must always start with `c-` and contain only lowecase letters, numbers and dashes.

## Dialogue: general idea

In general, a dialogue can contain two types of objects

- a **question**: even if it's often the case, it's not mandatory for a question to require an answer. In Kastle, a question is
  just a phrase spoken by a character different from the player
- an **answer**: every question except the final ones carries one or more answers, that are phrases said by the player

Each question object carries its own answers and each answer references the next question by its ID. The dialogue ends when the question does not have answers or has a reward (an item that the player earns from the dialogue).

!!! info "Question ID"

    The ID of any question must always start with `d-` and contain only lowecase letters, numbers and dashes.

## Defining a dialogue

A dialogue always starts with a `dialogue` block inside the `character` block.
The absolute first thing that we should do after we instantiated the `dialogue` is to instantiate a question.
The first question must be declared using the `firstQuestion` function, all the subsequent ones should use `question`,
but they work exactly the same.

- First of all you need to define the text of the question through the `text` property
- You can add answers by using the `answer` function as many times as you want. To define the answer:
    - Define the text through the property `text`
    - Define the next question through the property `nextQuestion` (referenced by ID)
- If you don't add any answers, the question is the last one in the dialogue. In this case you can define a reward through
the `reward` function, that works exactly like instantiating an item. 

Let's review the complete example that we did in [Exapanding the game](expanding.md):
```kotlin
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
                
                You are ready for a ton of fun playing Kastle games!'
            """.trimIndent()
        }
    }

    question("d-ifno") {
        text = "No problem, I'll wait until you're ready, but you're missing a lot of fun!"
    }
}
```