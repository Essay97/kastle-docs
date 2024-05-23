# Project setup

A Kastle game is basically a Kotlin project. If you are already familiar with Kotlin, you can go on and create a Gradle project as you usally do.
If this is your first time writing a Kotlin project, you'll probably need the IntelliJ IDEA Community Edition IDE, since it gives the best developer
experience when writing Kotlin code.

You can follow the [official documentation](https://www.jetbrains.com/help/idea/get-started-with-kotlin.html#new-kotlin-project-no-frameworks) for the Kotlin
project setup.

## Installing Kastle dependencies

Once you created the project, go to your `build.gradle.kts` file and add the following line into the `dependencies` block and reload the Gradle project:

```kotlin hl_lines="2"
dependencies {
    implementation("io.github.essay97:kastle-lib:1.0.0")
}
```

## Implement the GameProvider

This last step makes the project an actual Kastle game: the dependency that we installed in the previous step allows access to the `GameProvider` interface,
that we need to implement.

The following is an example:

```kotlin
package com.example.mypackage // (1)!

import io.github.essay97.kastle.service.GameProvider

class ExampleGame : GameProvider {
    override fun provideConfiguration(): GameConfiguration {
        TODO("Implement your game here!")
    }
}
```

1. Substitute this with your own package name. Make it unique so that your class does not clash with the ones from
   other developers.

## Let the engine discover the game

Just implementing the GameProvider interface is not sufficient though. To make sure that the engine can discover your game,
you need to create a file in the `resources/META-INF/services` folder called `io.github.essay97.kastle.service.GameProvider`
and write a single line that is the fully qualified name of your class (`<package name>.<class name>`).

If we continue with the previous example, the folder structure would be:

```yaml
src
└── main
├── kotlin
│   └── ExampleGame.kt
└── resources
└── META-INF
└── services
└── io.github.essay97.kastle.service.GameProvider # (1)!
```

1. The content of this file is `com.example.mypackage.ExampleGame`
