# Gradle Files and Folder Structure

## What

A Gradle Java project consists of several files that replace the Maven `pom.xml`.
These files define the project name, plugins, dependencies, and how to run Gradle.

## Why

Gradle is an alternative build tool to Maven. Understanding its files and folder layout
is the basis for working with Gradle in any Java project.

## Example

In `backend/settings.gradle` the project name is defined. In `backend/build.gradle`
the plugins, group, Java version, repositories and dependencies are declared.
The `backend/gradlew` script is the entry point for running builds.

## Gotchas

- `gradlew` and `gradlew.bat` are **real files** (shell scripts / batch files), not just helpers.
  Under WSL/Windows they need execute permissions: `chmod +x gradlew`.
- The `gradle/wrapper/` directory contains the fixed Gradle version used for the build.
  This ensures all developers use the same Gradle version.
- `build/` and `.gradle/` are generated during the build. They are git-ignored.

## Used in

- `backend/settings.gradle` — project name
- `backend/build.gradle` — plugins, dependencies, build configuration
- `backend/gradlew` — Gradle Wrapper script for WSL/Linux
