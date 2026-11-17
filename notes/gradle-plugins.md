# Gradle Plugins

## What
What is the concept? Two or three sentences.
Plugins are used to extend the capabilities of gradle like
using java or spring boot web.

## Why
What problem does it solve?
It makes it possible to use java in the first place with gradle.
In addition it can be used to add functionality similiar to
the parent pom of spring boot web in maven which entiles
vesions and certain dependencies. But in contrast to maven
there is no one parent pom but to plugins to achieve this!

## Example
A practical example, preferably from this repository. Reference real paths.

plugins {
	id 'java'
	id 'org.springframework.boot' version '4.1.2'
	id 'io.spring.dependency-management' version '1.1.7'
}

Here without the first plugin 'java' we wouldn't be able to use
java with gradle. The second one adds versions and the third one dependencies.

## Gotchas
- 

## Used in
- /backend/build.gradle

