# Distributing your game

Distribute the game JAR and the fully qualified name of its `GameProvider` class. The JAR must contain both that class and `META-INF/services/com.saggiodev.kastle.service.GameProvider`, whose content is the provider class name.

For the [quickstart](project-setup.md), build with `./gradlew build` (`gradlew.bat build` on Windows). The output is `build/libs/tutorial-game-1.0.0.jar`. A normal Gradle JAR includes your compiled classes and resources, but does not bundle dependencies. The CLI supplies Kastle and its runtime dependencies; avoid relying on additional libraries unless you also arrange to package them.

Players install and launch it with:

```sh
kastle install com.example.mypackage.ExampleGame tutorial-game-1.0.0.jar --name tutorial
kastle play tutorial
```

The installation arguments are the provider class followed by the JAR path. `--name` (or `-n`) sets the installed name used by `play`; without it the name defaults to the JAR filename without its extension.

There is no built-in remote game repository. Share the JAR using your preferred distribution method and tell players which CLI build you tested. For a matching 0.1.2 CLI, see [Installing the engine](../getting-started/installation.md). For local API changes, build the CLI from the same checkout as described in [Project setup](project-setup.md).
