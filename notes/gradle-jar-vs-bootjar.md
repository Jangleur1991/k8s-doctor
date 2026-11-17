# Gradle: jar vs. bootJar

## What
Gradle produces two types of JAR artifacts: the standard `jar` task creates a lightweight JAR containing only your compiled classes (no dependencies). The Spring Boot `bootJar` task creates a "fat JAR" that includes your classes **plus all transitive dependencies**, a repackaged Manifest with `Main-Class` and `Class-Path` attributes, and nested JAR support.

## Why
Spring Boot applications need to run as a single executable artifact. A standard JAR lacks dependency JARs and the Spring Boot launcher configuration — `java -jar` on a plain JAR fails with "no main manifest attribute" or `ClassNotFoundException`. The `bootJar` artifact is what you deploy to Kubernetes.

## Example
```bash
./gradlew bootJar    # Produces build/libs/k8sdoctor-0.0.1-SNAPSHOT.jar (~50-100 MB, executable)
./gradlew jar        # Produces build/libs/k8sdoctor-0.0.1-SNAPSHOT.jar (~2 KB, not executable)
```

Verify with: `jar tf build/libs/*.jar | grep Main-Class` — only `bootJar` produces a repackaged JAR with a Spring Boot launcher class.

## Gotchas
- **Never deploy the plain `jar` to K8s.** It has no dependency JARs and no Spring Boot bootstrapping code.
- **Both tasks produce files with the same name** in `build/libs/` — running both overwrites the plain JAR, so always check the file size to confirm which one you have.
- The `bootJar` task is added by the `org.springframework.boot` plugin, **not** by `dependency-management`. The `dependency-management` plugin only handles version numbers.
- If you exclude a dependency in `build.gradle`, the `bootJar` will still include it unless you exclude at the task level too.
- The plain `jar` is still useful for library projects or as an intermediate artifact that `bootJar` depends on.

## Used in
- `backend/build.gradle` — `bootJar` and `jar` task declarations
- `backend/build/libs/` — generated JAR artifacts


