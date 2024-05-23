# Project setup

A Kastle game is basically a Kotlin project. If you are already familiar with Kotlin, you can go on and create a Gradle project as you usally do.
If this is your first time writing a Kotlin project, you'll probably need the IntelliJ IDEA Community Edition IDE, since it gives the best developer
experience when writing Kotlin code.

You can follow the [official documentation](https://www.jetbrains.com/help/idea/get-started-with-kotlin.html#new-kotlin-project-no-frameworks) for the Kotlin
project setup.

## Installing Kastle dependencies

Once you created the project, go to your `build.gradle.kts` file and add the following line into the `dependencies` block and reload the Gradle project:

```kotlin
implementation("io.github.essay97:kastle-lib:1.0.0")
```

## GameProvider

This last step makes the project an actual Kastle game: the dependency that we installed in the previous step allows access to the `GameProvider` interface,
that we need to implement.

The following is an example:

```kotlin
package com.example.mypackage

class MyKastleGame : GameProvider {

}
```
