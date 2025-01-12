# Maven Build & Publish GitLab CI/CD Component

This component provides a reusable pipeline for building and publishing Maven artifacts to a Nexus repository.

## Usage

Include this component in your GitLab CI/CD pipeline:

```yaml
include:
  - component: gitlab.com/your-group/maven-build-publish/maven-build-publish@1.0
    inputs:
      java_version: "17" # Optional, default is 17
      maven_version: "3.9.5" # Optional, default is 3.9.5
      maven_args: "" # Optional additional Maven arguments
      nexus_url: "https://your-nexus-server" # Required
      nexus_repo: "your-repo-name" # Required
      nexus_username: "$NEXUS_USER" # Required, recommend using CI/CD variables
      nexus_password: "$NEXUS_PASSWORD" # Required, recommend using CI/CD variables
```

## Inputs

| Input          | Required | Default | Description |
|----------------|----------|---------|-------------|
| java_version   | No       | 17      | Java version to use |
| maven_version  | No       | 3.9.5   | Maven version to use |
| maven_args     | No       | ""      | Additional Maven arguments |
| nexus_url      | Yes      | -       | Nexus repository URL |
| nexus_repo     | Yes      | -       | Nexus repository name |
| nexus_username | Yes      | -       | Nexus username |
| nexus_password | Yes      | -       | Nexus password |

## Example

```yaml
include:
  - component: gitlab.com/your-group/maven-build-publish/maven-build-publish@1.0
    inputs:
      java_version: "11"
      maven_version: "3.8.6"
      maven_args: "-DskipTests"
      nexus_url: "https://nexus.example.com"
      nexus_repo: "maven-releases"
      nexus_username: "$NEXUS_USER"
      nexus_password: "$NEXUS_PASSWORD"
```

## Publishing

Artifacts are automatically published when creating Git tags. The version will be derived from the tag name.
