# Gradle Toolchain

## What

A toolchain specifies which JDK Gradle should use to compile the project code.
It is independent of the JDK used to run the Gradle process itself.

## Why

Gradle always needs an JDK to run. But the project may require a different version.
The toolchain tells Gradle to use that version for compilation, even if a different
JDK started Gradle. This ensures all developers and build servers compile with the
same Java version.

## Example

```
java {
    toolchain {
        languageVersion = JavaLanguageVersion.of(25)
    }
}
```

This entry in `backend/build.gradle` required a JDK 25 on every machine that cloned
the project. Without it, `./gradlew build` failed.

## Gotchas

- The toolchain demands the **exact** version. JDK 21 does not replace JDK 25.
- Gradle needs a download repository configured to auto-provision a missing JDK.
  Without one, a missing JDK is a hard failure.
- If no toolchain is specified, Gradle uses the JDK that runs the Gradle process.
- The toolchain is not needed if the Gradle process JDK already matches the project.

## Used in

- `backend/build.gradle` — Java toolchain configuration
