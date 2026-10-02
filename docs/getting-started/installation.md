# Installing the engine

Kastle runs on **Java 11 or later**, with `java` available in your system's `PATH`.

These guides describe Kastle 0.1.2. The API artifact is published on Maven Central, but CLI downloads are published separately. A hosted CLI archive could not be verified for this documentation update; the source-build instructions below produce a matching CLI.

## Build Kastle 0.1.2

Building requires Git, a JDK to run Gradle (JDK 17 recommended), and an installed JDK 11 toolchain for compilation. Gradle itself is supplied by the repository's wrapper.

```sh
git clone --branch v0.1.2 https://github.com/Essay97/kastle-monorepo.git
cd kastle-monorepo
./gradlew :engine:installDist
```

On Windows, use `gradlew.bat :engine:installDist` instead. The launchers are:

- macOS/Linux: `engine/build/install/kastle/bin/kastle`
- Windows: `engine/build/install/kastle/bin/kastle.bat`

Run the launcher with `--help` to verify the installation. You can add its `bin` directory to your `PATH` to use `kastle` from any terminal. Keep the adjacent `lib` directory with the launcher; it contains the runtime dependencies.

## Use a distribution archive

If you already have a compatible Kastle ZIP, extract it and open `kastle-<version>/bin`. Use `kastle` on macOS/Linux or `kastle.bat` on Windows. Add that `bin` directory to your `PATH`, or invoke the launcher by its path.

To produce the ZIP yourself from the checkout above, run `./gradlew :engine:distZip` (`gradlew.bat :engine:distZip` on Windows). The result is `engine/build/distributions/kastle-0.1.2.zip`.

Next, [install a game](install-games.md).
