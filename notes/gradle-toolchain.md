# Gradle Toolchain

## What
What is the concept? Two or three sentences.
With toolchain you can set the java version used by the compiler.

## Why
What problem does it solve?
Gradle itself uses a jdk. So without a jdk you can not use gradle.
But some projects need a newer jdk version. In order to get this information
you can run gradlew build, which works for any jdk, but you will get a 
error message in case you do not use the toolchain jdk version.

## Example

java {
	toolchain {
		languageVersion = JavaLanguageVersion.of(25)
	}
}


## Gotchas
- 

## Used in
- /backend/build.gradle
