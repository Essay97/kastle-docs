# Project setup

A Kastle game is a Kotlin/JVM project that implements `GameProvider`. Create a Gradle Kotlin project with a Gradle wrapper (the monorepo uses Gradle 8.10). Set `rootProject.name = "tutorial-game"` in `settings.gradle.kts` to use the JAR names shown in this guide.

## Configuring Gradle

The monorepo uses Kotlin 2.1.10 and targets Java 11. Use the same JVM target for games intended to run on Java 11. Put this in `build.gradle.kts`:

```kotlin
plugins {
    kotlin("jvm") version "2.1.10"
}

group = "com.example"
version = "1.0.0"

repositories {
    mavenCentral()
}

kotlin {
    jvmToolchain(11)
}

dependencies {
    implementation("com.saggiodev:kastle-api:0.1.2")
}
```

API `0.1.2` is published on Maven Central. The monorepo takes its version from `gradle.properties`, and all modules inherit it. API and CLI publication are separate, so a published API does not imply an available CLI download. For a matching CLI, follow [Installing the engine](../getting-started/installation.md). See the [release policy](https://github.com/Essay97/kastle-monorepo/blob/main/RELEASING.md) for versioning and publication details.

These guides describe Kastle 0.1.2, including automatic name matching, terminal dialogue rewards, and game-definition validation. To develop against a local monorepo checkout instead of the published API, optionally substitute the dependency in the game's `settings.gradle.kts`:

```kotlin
rootProject.name = "tutorial-game"

includeBuild("../kastle-monorepo") {
    dependencySubstitution {
        substitute(module("com.saggiodev:kastle-api")).using(project(":api"))
    }
}
```

Adjust the path to your checkout. From the monorepo, run `./gradlew :engine:installDist` to build the matching CLI at `engine/build/install/kastle/bin/kastle` (`kastle.bat` on Windows).

## Implement the GameProvider

Create `src/main/kotlin/ExampleGame.kt`:

```kotlin
package com.example.mypackage

import com.saggiodev.kastle.dto.GameConfiguration
import com.saggiodev.kastle.service.GameProvider

class ExampleGame : GameProvider {
    override fun provideConfiguration(): GameConfiguration {
        TODO("Implement your game here!")
    }
}
```

Replace the package with your own unique package name. The [next page](first-game.md) supplies a complete configuration.

## Let the engine discover the game

Create the UTF-8 text file `src/main/resources/META-INF/services/com.saggiodev.kastle.service.GameProvider` containing this line:

```text
com.example.mypackage.ExampleGame
```

This is the fully qualified name of your provider class, not the filename. The public class must be instantiable without constructor arguments by Java's `ServiceLoader`.

```text
src/main/
├── kotlin/
│   └── ExampleGame.kt
└── resources/
    └── META-INF/
        └── services/
            └── com.saggiodev.kastle.service.GameProvider
```

The current publishing configuration uses `emptyDocs()`, so the navigation links to the [API source and KDoc](https://github.com/Essay97/kastle-monorepo/tree/main/api/src/main/kotlin) rather than promising generated API documentation.

## Definition validation

When a game is launched, Kastle calls `provideConfiguration()` and validates its definition before updating runtime registries. Definition errors are reported together with context so you can fix the affected rooms, items, characters, or questions.

- Room, item, and character IDs must be valid and unique within their respective definitions. Reward items belong to the same item definitions as ordinary room items, so reward IDs must not collide with them or other reward definitions.
- The initial room, room contents, link destinations, trigger items, and winning-condition references must exist in this configuration.
- Question IDs must be unique within each character's dialogue. The first question and every answer's destination must exist in that dialogue. Dialogues cannot contain cycles, including self-loops or cycles in unreachable questions; multiple branches may share a later question.
- Rewards are allowed only on terminal questions and must reference defined items. In the DSL, omit answers to make a terminal question. With direct DTOs, use `answers = null`; a non-null empty answer list is invalid.

Installation checks provider packaging and construction, not the configuration returned by the provider. A successful install therefore does not replace launching and testing your game.
