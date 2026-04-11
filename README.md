# mudman-reusable

A collection of reusable GitHub Actions workflows and composite actions to standardize CI/CD automation across multiple repositories.

## ⚠️ Important: Version Pinning

For security and stability, **always pin to a specific release tag or commit SHA** when using these workflows and actions in production. Using `@main` can expose your CI/CD pipeline to unexpected changes or security vulnerabilities.

**Recommended:**
```yaml
uses: mudman1986/mudman-reusable/.github/workflows/reusable-super-linter.yml@v1.0.0  # Pin to release tag
uses: mudman1986/mudman-reusable/.github/workflows/reusable-super-linter.yml@a1b2c3d  # Pin to commit SHA
```

**Not recommended for production:**
```yaml
uses: mudman1986/mudman-reusable/.github/workflows/reusable-super-linter.yml@main  # Development only
```

## 📋 Table of Contents

- [Reusable Workflows](#reusable-workflows)
  - [Super-Linter](#super-linter)
 
## 🔄 Reusable Workflows

### Super-Linter

Lint your codebase using Super-Linter with customizable configuration.

**File:** `.github/workflows/reusable-super-linter.yml`

**Usage:**

```yaml
name: Lint Code

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  lint:
    uses: mudman1986/mudman-reusable/.github/workflows/reusable-super-linter.yml@v1.0.0
    with:
      validate-all-codebase: false
      default-branch: main
```

**Inputs:**

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `validate-all-codebase` | Validate all codebase or only changed files | No | `false` |
| `default-branch` | The default branch name | No | `main` |
| `linter-rules-path` | Directory for linter configuration files | No | `.` |
| `filter-regex-exclude` | Regular expression for excluding files/folders | No | `''` |
| `javascript-es-config-file` | Path to a custom ESLint config used for JavaScript/TypeScript; override this when your project does not use the default `.eslintrc.yml` | No | `.eslintrc.yml` |
| `validate-javascript-standard` | Enable JavaScript validation using the StandardJS ruleset; set to `false` if you only want to use your own ESLint configuration | No | `true` |
| `validate-typescript-standard` | Enable TypeScript validation using the StandardJS ruleset; set to `false` if TypeScript is linted exclusively via your custom ESLint configuration | No | `true` |

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📝 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 📧 Contact

For questions or support, please open an issue in this repository.