# Quick Start Guide

This guide helps you quickly integrate reusable workflows and composite actions from this repository into your projects.

## 🚀 Getting Started

### 1. Add Super-Linter to Your Repository

Create `.github/workflows/lint.yml`:

```yaml
name: Lint Code Base

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  lint:
    uses: mudman1986/mudman-reusable/.github/workflows/reusable-super-linter.yml@main
    with:
      validate-all-codebase: false
      default-branch: main
```

### 2. Add Security Scanning with CodeQL

Create `.github/workflows/security.yml`:

```yaml
name: Security Scan

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]
  schedule:
    - cron: '0 0 * * 1'  # Weekly on Mondays

jobs:
  codeql:
    uses: mudman1986/mudman-reusable/.github/workflows/reusable-codeql-analysis.yml@main
    with:
      languages: '["javascript"]'  # Adjust based on your project
```

### 3. Add Dependency Review for Pull Requests

Create `.github/workflows/dependency-review.yml`:

```yaml
name: Dependency Review

on:
  pull_request:
    branches: [ main ]

jobs:
  review:
    uses: mudman1986/mudman-reusable/.github/workflows/reusable-dependency-review.yml@main
    with:
      fail-on-severity: moderate
```

### 4. Add Automated Testing

Create `.github/workflows/test.yml`:

```yaml
name: Test

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    uses: mudman1986/mudman-reusable/.github/workflows/reusable-test-suite.yml@main
    with:
      node-version: '20'
      test-command: npm test
      install-command: npm ci
```

### 5. Add Docker Image Building (Optional)

Create `.github/workflows/docker.yml`:

```yaml
name: Docker

on:
  push:
    branches: [ main ]
    tags: [ 'v*' ]

jobs:
  build:
    uses: mudman1986/mudman-reusable/.github/workflows/reusable-docker-build.yml@main
    with:
      image-name: ${{ github.repository }}
      platforms: linux/amd64,linux/arm64
      push: true
    secrets:
      registry-password: ${{ secrets.GITHUB_TOKEN }}
```

## 📦 Using Composite Actions

### In Your Workflow Files

Replace standard checkout and setup steps with our composite actions:

**Before:**
```yaml
steps:
  - uses: actions/checkout@v4
  - uses: actions/setup-node@v4
    with:
      node-version: '20'
      cache: 'npm'
```

**After:**
```yaml
steps:
  - uses: mudman1986/mudman-reusable/.github/actions/checkout-with-cache@main
  - uses: mudman1986/mudman-reusable/.github/actions/setup-node-with-cache@main
    with:
      node-version: '20'
```

## 🎯 Common Configurations

### JavaScript/TypeScript Project

```yaml
name: CI

on: [push, pull_request]

jobs:
  lint:
    uses: mudman1986/mudman-reusable/.github/workflows/reusable-super-linter.yml@main

  security:
    uses: mudman1986/mudman-reusable/.github/workflows/reusable-codeql-analysis.yml@main
    with:
      languages: '["javascript"]'

  test:
    uses: mudman1986/mudman-reusable/.github/workflows/reusable-test-suite.yml@main
    with:
      node-version: '20'
      test-command: npm test
```

### Python Project

```yaml
name: CI

on: [push, pull_request]

jobs:
  lint:
    uses: mudman1986/mudman-reusable/.github/workflows/reusable-super-linter.yml@main

  security:
    uses: mudman1986/mudman-reusable/.github/workflows/reusable-codeql-analysis.yml@main
    with:
      languages: '["python"]'

  test:
    uses: mudman1986/mudman-reusable/.github/workflows/reusable-test-suite.yml@main
    with:
      python-version: '3.11'
      test-command: pytest
      install-command: pip install -r requirements.txt
```

### Multi-Language Project

```yaml
name: CI

on: [push, pull_request]

jobs:
  lint:
    uses: mudman1986/mudman-reusable/.github/workflows/reusable-super-linter.yml@main

  security:
    uses: mudman1986/mudman-reusable/.github/workflows/reusable-codeql-analysis.yml@main
    with:
      languages: '["javascript", "python"]'
```

## 🔧 Customization Tips

### Super-Linter Configuration

Add a `.github/linters/` directory with configuration files:
- `.eslintrc.yml` - JavaScript/TypeScript linting
- `.python-lint` - Python linting
- `.markdown-lint.yml` - Markdown linting

### CodeQL Custom Queries

Create `.github/codeql/codeql-config.yml`:

```yaml
name: "CodeQL Config"
queries:
  - uses: security-and-quality
  - uses: security-extended
```

Then reference it:

```yaml
jobs:
  codeql:
    uses: mudman1986/mudman-reusable/.github/workflows/reusable-codeql-analysis.yml@main
    with:
      languages: '["javascript"]'
      config-file: .github/codeql/codeql-config.yml
```

## 📚 Additional Resources

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Super-Linter Documentation](https://github.com/super-linter/super-linter)
- [CodeQL Documentation](https://codeql.github.com/docs/)
- [Dependency Review Action](https://github.com/actions/dependency-review-action)

## 💡 Best Practices

1. **Start Simple**: Begin with just linting and security scanning
2. **Add Gradually**: Introduce testing and building workflows as needed
3. **Use Branch Protection**: Require workflows to pass before merging
4. **Pin Versions**: Use `@v1` or `@main` based on your stability needs
5. **Monitor Usage**: Review Actions usage in repository insights
6. **Keep Updated**: Regularly update to latest workflow versions

## 🐛 Troubleshooting

### Workflow Not Found
- Ensure the repository path is correct: `mudman1986/mudman-reusable`
- Verify you're using the correct branch (usually `@main`)

### Permission Errors
- Check that workflows have the necessary permissions
- Verify `GITHUB_TOKEN` has appropriate scopes

### Linter Failures
- Review Super-Linter output for specific file issues
- Add exclusions using `filter-regex-exclude` if needed

## 📞 Support

For issues or questions:
1. Check the main [README.md](README.md) documentation
2. Review workflow files for detailed configuration options
3. Open an issue in this repository
