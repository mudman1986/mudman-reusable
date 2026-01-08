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
  - [CodeQL Security Scan](#codeql-security-scan)
  - [Dependency Review](#dependency-review)
  - [Test Suite](#test-suite)
  - [Docker Build and Push](#docker-build-and-push)
  - [Release Automation](#release-automation)
- [Composite Actions](#composite-actions)
  - [Checkout with Cache](#checkout-with-cache)
  - [Setup Node.js with Cache](#setup-nodejs-with-cache)
  - [Setup Python with Cache](#setup-python-with-cache)
- [Usage Examples](#usage-examples)

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

### CodeQL Security Scan

Perform security analysis using GitHub CodeQL.

**File:** `.github/workflows/reusable-codeql-analysis.yml`

**Usage:**

```yaml
name: CodeQL Analysis

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]
  schedule:
    - cron: '0 0 * * 1'  # Weekly on Mondays

jobs:
  analyze:
    uses: mudman1986/mudman-reusable/.github/workflows/reusable-codeql-analysis.yml@v1.0.0
    with:
      languages: '["javascript", "python"]'
      queries: security-and-quality
```

**Inputs:**

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `languages` | Languages to analyze (JSON array format) | No | `["javascript"]` |
| `queries` | CodeQL query suite to use | No | `security-and-quality` |
| `config-file` | Path to CodeQL config file | No | `''` |

---

### Dependency Review

Review dependency changes for security vulnerabilities and license compliance.

**File:** `.github/workflows/reusable-dependency-review.yml`

**Usage:**

```yaml
name: Dependency Review

on:
  pull_request:
    branches: [ main ]

jobs:
  review:
    uses: mudman1986/mudman-reusable/.github/workflows/reusable-dependency-review.yml@v1.0.0
    with:
      fail-on-severity: moderate
      fail-on-scopes: runtime
```

**Inputs:**

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `fail-on-severity` | Fail if vulnerability with this severity or higher is detected | No | `low` |
| `allow-licenses` | Comma-separated list of allowed licenses | No | `''` |
| `deny-licenses` | Comma-separated list of denied licenses | No | `''` |
| `fail-on-scopes` | Comma-separated list of dependency scopes to fail on | No | `runtime` |

---

### Test Suite

Run tests with configurable Node.js or Python environments.

**File:** `.github/workflows/reusable-test-suite.yml`

**Usage:**

```yaml
name: Run Tests

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    uses: mudman1986/mudman-reusable/.github/workflows/reusable-test-suite.yml@v1.0.0
    with:
      node-version: '20'
      test-command: npm test
      install-command: npm ci
```

**Inputs:**

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `runner` | GitHub runner to use | No | `ubuntu-latest` |
| `node-version` | Node.js version (leave empty to skip setup) | No | `''` |
| `python-version` | Python version (leave empty to skip setup) | No | `''` |
| `test-command` | Command to run tests | Yes | - |
| `install-command` | Command to install dependencies | No | `npm ci` |
| `working-directory` | Working directory for commands | No | `.` |

---

### Docker Build and Push

Build and push Docker images to container registries.

**File:** `.github/workflows/reusable-docker-build.yml`

**Usage:**

```yaml
name: Build Docker Image

on:
  push:
    branches: [ main ]
    tags: [ 'v*' ]

jobs:
  docker:
    uses: mudman1986/mudman-reusable/.github/workflows/reusable-docker-build.yml@v1.0.0
    with:
      image-name: ${{ github.repository }}
      platforms: linux/amd64,linux/arm64
      push: true
    secrets:
      registry-username: ${{ github.actor }}
      registry-password: ${{ secrets.GITHUB_TOKEN }}
```

**Inputs:**

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `image-name` | Docker image name | Yes | - |
| `registry` | Container registry | No | `ghcr.io` |
| `context` | Build context path | No | `.` |
| `dockerfile` | Path to Dockerfile | No | `Dockerfile` |
| `platforms` | Target platforms | No | `linux/amd64` |
| `push` | Whether to push the image | No | `true` |
| `tags` | Additional tags (comma-separated) | No | `''` |

**Secrets:**

| Secret | Description | Required |
|--------|-------------|----------|
| `registry-username` | Registry username | No (defaults to `github.actor`) |
| `registry-password` | Registry password or token | No (defaults to `GITHUB_TOKEN`) |

---

### Release Automation

Automatically create GitHub releases with changelog generation when tags are pushed.

**File:** `.github/workflows/reusable-release-automation.yml`

**Usage:**

```yaml
name: Release

on:
  push:
    tags:
      - 'v*'

jobs:
  release:
    uses: mudman1986/mudman-reusable/.github/workflows/reusable-release-automation.yml@v1.0.0
    with:
      create-changelog: true
      draft: false
```

**Inputs:**

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `tag-pattern` | Git tag pattern to trigger release | No | `v*` |
| `create-changelog` | Automatically generate changelog from commits | No | `true` |
| `draft` | Create release as draft | No | `false` |
| `prerelease` | Mark release as prerelease | No | `false` |
| `release-notes-file` | Path to release notes file (if not auto-generating) | No | `''` |

---

## 🎯 Composite Actions

### Checkout with Cache

Enhanced checkout action with support for submodules and Git LFS.

**File:** `.github/actions/checkout-with-cache/action.yml`

**Usage:**

```yaml
steps:
  - uses: mudman1986/mudman-reusable/.github/actions/checkout-with-cache@v1.0.0
    with:
      fetch-depth: 0
      submodules: true
```

**Inputs:**

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `fetch-depth` | Number of commits to fetch | No | `1` |
| `submodules` | Whether to checkout submodules | No | `false` |
| `lfs` | Whether to download Git-LFS files | No | `false` |
| `token` | Personal access token (PAT) | No | `${{ github.token }}` |

---

### Setup Node.js with Cache

Setup Node.js with automatic dependency caching.

**File:** `.github/actions/setup-node-with-cache/action.yml`

**Usage:**

```yaml
steps:
  - uses: mudman1986/mudman-reusable/.github/actions/setup-node-with-cache@v1.0.0
    with:
      node-version: '20'
      cache: npm
```

**Inputs:**

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `node-version` | Node.js version to use | No | `20` |
| `cache` | Package manager for caching (npm, yarn, pnpm) | No | `npm` |
| `cache-dependency-path` | Path to dependency file(s) for cache key | No | `''` |
| `registry-url` | Optional registry URL for auth | No | `''` |

---

### Setup Python with Cache

Setup Python with automatic dependency caching.

**File:** `.github/actions/setup-python-with-cache/action.yml`

**Usage:**

```yaml
steps:
  - uses: mudman1986/mudman-reusable/.github/actions/setup-python-with-cache@v1.0.0
    with:
      python-version: '3.11'
      cache: pip
```

**Inputs:**

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `python-version` | Python version to use | No | `3.11` |
| `cache` | Package manager for caching (pip, pipenv, poetry) | No | `pip` |
| `cache-dependency-path` | Path to dependency file(s) for cache key | No | `''` |
| `architecture` | Python architecture (x86, x64) | No | `x64` |

---

## 💡 Usage Examples

### Complete CI/CD Pipeline

Here's a complete example combining multiple reusable workflows:

```yaml
name: CI/CD Pipeline

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

  security:
    uses: mudman1986/mudman-reusable/.github/workflows/reusable-codeql-analysis.yml@v1.0.0
    with:
      languages: '["javascript"]'

  dependency-review:
    if: github.event_name == 'pull_request'
    uses: mudman1986/mudman-reusable/.github/workflows/reusable-dependency-review.yml@v1.0.0
    with:
      fail-on-severity: moderate

  test:
    uses: mudman1986/mudman-reusable/.github/workflows/reusable-test-suite.yml@v1.0.0
    with:
      node-version: '20'
      test-command: npm test
      install-command: npm ci

  docker:
    if: github.event_name == 'push' && github.ref == 'refs/heads/main'
    needs: [lint, security, test]
    uses: mudman1986/mudman-reusable/.github/workflows/reusable-docker-build.yml@v1.0.0
    with:
      image-name: ${{ github.repository }}
      push: true
    secrets:
      registry-password: ${{ secrets.GITHUB_TOKEN }}
```

### Using Composite Actions

```yaml
name: Build and Test

on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: mudman1986/mudman-reusable/.github/actions/checkout-with-cache@v1.0.0
        with:
          fetch-depth: 0

      - uses: mudman1986/mudman-reusable/.github/actions/setup-node-with-cache@v1.0.0
        with:
          node-version: '20'
          cache: npm

      - name: Install dependencies
        run: npm ci

      - name: Build
        run: npm run build

      - name: Test
        run: npm test
```

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📝 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 📧 Contact

For questions or support, please open an issue in this repository.