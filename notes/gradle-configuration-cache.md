# Gradle: Configuration Cache

## What
The **Configuration Cache** (`--configuration-cache`) serializes Gradle's entire build configuration state after the first build — including parsed `build.gradle` scripts, resolved plugins, computed task graphs, and dependency metadata. Subsequent builds load this cached state directly, skipping the parsing and configuration phase entirely.

## Why
Gradle must parse `build.gradle`, load all plugins, resolve dependency coordinates against Maven Central, and build the task DAG on every build. This takes 1–3 seconds per build even when no code has changed. The Configuration Cache turns this into a near-instant load, speeding up repeated builds dramatically.

## Example
Enable globally in `~/.gradle/gradle.properties`:
```properties
org.gradle.configuration-cache=true
```
Or run once: `./gradlew build --configuration-cache`

First run: parses everything, stores cache in `~/.gradle/configuration-cache/`.
Second run: reads cache from disk, skips parsing, runs only the changed tasks.

## Gotchas
- **Invalidation is automatic but all-or-nothing.** Any change to `build.gradle`, `settings.gradle`, or any file included by `build.gradle` (e.g., `buildSrc/`) invalidates the entire cache. Gradle does not cache partial configurations.
- **The cache is per-project, not global.** Each project directory has its own cache key based on the hash of all configuration inputs.
- **Not all plugins support it.** Third-party plugins may use Gradle internals that break cache serialization. If a build fails with `--configuration-cache`, the plugin is likely incompatible — remove the flag and report the issue.
- **The flag name is misleading.** It caches the *configuration phase* (reading `build.gradle`, resolving plugins, building the task DAG), **not** the task execution. Tasks like `compileJava` still re-run if source files changed.
- **The cache is stored on disk in `~/.gradle/configuration-cache/`.** It is human-readable (JSON) and can be inspected for debugging.
- **Configuration Cache and Build Scan do not work together in all Gradle versions.** Check compatibility with your Gradle version.


