# Node.js Build & Publish GitLab CI/CD Component

This component provides a reusable pipeline for building and publishing Node.js packages to a Nexus repository.

## Usage

Include this component in your GitLab CI/CD pipeline:

```yaml
include:
  - component: gitlab.com/your-group/nodejs-build-publish/nodejs-build-publish@1.0
    inputs:
      node_version: "20" # Optional, default is 20
      package_manager: "npm" # Optional, default is npm (options: npm, yarn)
      build_args: "" # Optional additional build arguments
      nexus_url: "https://your-nexus-server" # Required
      nexus_repo: "your-repo-name" # Required
      nexus_username: "$NEXUS_USER" # Required, recommend using CI/CD variables
      nexus_password: "$NEXUS_PASSWORD" # Required, recommend using CI/CD variables
```

## Inputs

| Input          | Required | Default | Description |
|----------------|----------|---------|-------------|
| node_version   | No       | 20      | Node.js version to use |
| package_manager| No       | npm     | Package manager to use (npm or yarn) |
| build_args     | No       | ""      | Additional build arguments |
| nexus_url      | Yes      | -       | Nexus repository URL |
| nexus_repo     | Yes      | -       | Nexus repository name |
| nexus_username | Yes      | -       | Nexus username |
| nexus_password | Yes      | -       | Nexus password |

## Example

```yaml
include:
  - component: gitlab.com/your-group/nodejs-build-publish/nodejs-build-publish@1.0
    inputs:
      node_version: "18"
      package_manager: "yarn"
      build_args: "--production"
      nexus_url: "https://nexus.example.com"
      nexus_repo: "npm-releases"
      nexus_username: "$NEXUS_USER"
      nexus_password: "$NEXUS_PASSWORD"
```

## Publishing

Artifacts are automatically published when creating Git tags. The version will be derived from the package.json version.

## Package Configuration

Your package.json must include the appropriate publish configuration:

```json
{
  "publishConfig": {
    "registry": "https://your-nexus-server/repository/your-repo-name/"
  }
}
```

For Yarn, add this to your .yarnrc.yml:

```yaml
npmRegistryServer: "https://your-nexus-server/repository/your-repo-name/"
npmAuthToken: "$NEXUS_PASSWORD"
