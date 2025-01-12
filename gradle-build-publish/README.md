# Gradle Build & Publish GitLab CI/CD Component

This component provides a reusable pipeline for building and publishing Gradle artifacts to a Nexus repository.

## Usage

Include this component in your GitLab CI/CD pipeline:

```yaml
include:
  - component: gitlab.com/your-group/gradle-build-publish/gradle-build-publish@1.0
    inputs:
      java_version: "17" # Optional, default is 17
      gradle_version: "8.5" # Optional, default is 8.5
      gradle_args: "" # Optional additional Gradle arguments
      nexus_url: "https://your-nexus-server" # Required
      nexus_repo: "your-repo-name" # Required
      nexus_username: "$NEXUS_USER" # Required, recommend using CI/CD variables
      nexus_password: "$NEXUS_PASSWORD" # Required, recommend using CI/CD variables
```

## Inputs

| Input          | Required | Default | Description |
|----------------|----------|---------|-------------|
| java_version   | No       | 17      | Java version to use |
| gradle_version | No       | 8.5     | Gradle version to use |
| gradle_args    | No       | ""      | Additional Gradle arguments |
| nexus_url      | Yes      | -       | Nexus repository URL |
| nexus_repo     | Yes      | -       | Nexus repository name |
| nexus_username | Yes      | -       | Nexus username |
| nexus_password | Yes      | -       | Nexus password |

## Example

```yaml
include:
  - component: gitlab.com/your-group/gradle-build-publish/gradle-build-publish@1.0
    inputs:
      java_version: "11"
      gradle_version: "7.6"
      gradle_args: "--no-daemon"
      nexus_url: "https://nexus.example.com"
      nexus_repo: "gradle-releases"
      nexus_username: "$NEXUS_USER"
      nexus_password: "$NEXUS_PASSWORD"
```

## Publishing

Artifacts are automatically published when creating Git tags. The version will be derived from the tag name.

## Gradle Configuration

Your build.gradle must include the maven-publish plugin and appropriate publishing configuration:

```groovy
plugins {
    id 'maven-publish'
}

publishing {
    publications {
        mavenJava(MavenPublication) {
            from components.java
        }
    }
    repositories {
        maven {
            url = System.getProperty('nexusUrl')
            credentials {
                username = System.getProperty('nexusUsername')
                password = System.getProperty('nexusPassword')
            }
        }
    }
}
