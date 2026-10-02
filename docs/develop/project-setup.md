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
    implementation("com.saggiodev:kastle-api:0.1.1")
}
```

The dependency coordinates and version above match `api/build.gradle.kts`. The API and CLI currently have separate version values: API `0.1.1`, CLI `0.1.0`. A version in the source does not guarantee that every change in that checkout is available in a published artifact.

These guides describe the current monorepo, including automatic name matching and terminal dialogue rewards. To use these changes before a release containing them is published, build the API and CLI from the same checkout. In the game's `settings.gradle.kts`, substitute the API dependency with the local project:

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
