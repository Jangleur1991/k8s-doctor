# Gradle Plugins

## What

Plugins extend Gradle with capabilities. They add build tasks, default configurations,
and dependency version management.

## Why

Maven has the `pom.xml` with built-in support for Java builds and a parent POM that
provides default settings and version management. Gradle needs explicit plugins for
the same. The `plugins` block in `build.gradle` replaces Maven's parent POM and
adds the required functionality.

## Example

```
plugins {
    id 'java'
    id 'org.springframework.boot' version '4.1.2'
    id 'io.spring.dependency-management' version '1.1.7'
}
```

- `java` — enables Java compilation and testing (Maven has this built-in, Gradle needs the plugin).
- `org.springframework.boot` — configures how Spring Boot JARs are built.
- `io.spring.dependency-management` — manages dependency versions. Spring Boot provides
  the correct versions automatically, so you do not need to specify them in each dependency.

## Gotchas

- `dependency-management` only handles **versions**, not adding dependencies.
  Dependencies still need to be listed in the `dependencies` block.
- The `java` plugin is separate from Spring Boot. Without it, Gradle cannot compile Java.
- Plugins are versioned. A plugin update may change default behaviour.

## Used in

- `backend/build.gradle` — plugin declarations
