# Gradle: Tasks

## What
A Gradle **task** is the smallest unit of work in a build — a single action like compiling, testing, or copying files. Tasks are organized into a directed acyclic graph (DAG): tasks declare their dependencies on other tasks, and Gradle executes them in the correct order.

## Why
Maven has fixed lifecycle phases (`validate` → `compile` → `test` → `package` → `install`). Gradle gives you granular control: you can run individual tasks independently, skip unnecessary steps, and compose complex builds from small reusable units. This makes builds faster and more predictable.

## Example
The `build` task is a composite task that delegates to:
- `assemble` (builds artifacts: `bootJar` + `jar`)
- `check` (runs checks: `test`)

Run sub-tasks directly:
```bash
./gradlew bootJar          # Build the executable JAR only
./gradlew test             # Run tests only
./gradlew clean bootJar    # Clean + bootJar, skip tests
./gradlew bootRun          # Compile and run the app (no JAR)
```

List all available tasks: `./gradlew tasks --all`

## Gotchas
- **Tasks are lazy.** Declaring them does not run them. Only the task name on the command line triggers execution; all other tasks it depends on run automatically.
- **Re-running without changes is fast.** Gradle marks unchanged tasks as `UP-TO-DATE` — they skip execution. Only changed inputs trigger recompilation/repacking.
- **`build` is not a magic command.** It is a convention that bundles `assemble` + `check`. If you only need the JAR, `bootJar` is faster because it skips `test` and `check`.
- **Plugins add tasks.** The `java` plugin adds `compileJava`, `processResources`, `test`. The `spring-boot` plugin adds `bootJar`, `bootRun`, `bootBuildImage`. Some tasks are hidden unless you use `--all`.
- **Order matters in the DAG.** You cannot run `bootJar` before `compileJava` — Gradle enforces task dependencies. But you can run `test` without `bootJar`.

## Used in
- `backend/build.gradle` — task configuration and dependency declarations
- `backend/gradlew` — Gradle Wrapper entry point 
