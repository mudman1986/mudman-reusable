# Examples

This directory contains example workflow files demonstrating how to use the reusable workflows and composite actions from this repository.

## 📁 Files

### [simple-ci.yml](simple-ci.yml)
The simplest possible setup - just linting, security scanning, and testing.

**Best for:** Getting started quickly, small projects

### [javascript-ci.yml](javascript-ci.yml)
Complete CI/CD pipeline for JavaScript/TypeScript projects.

**Best for:** Node.js, React, Vue, Angular, or any JavaScript/TypeScript project

**Includes:**
- Code linting with Super-Linter
- Security scanning with CodeQL
- Dependency review on pull requests
- Automated testing
- Docker image building and publishing

### [python-ci.yml](python-ci.yml)
Complete CI/CD pipeline for Python projects.

**Best for:** Django, Flask, FastAPI, or any Python project

**Includes:**
- Code linting with Super-Linter
- Security scanning with CodeQL
- Dependency review on pull requests
- Automated testing with pytest
- Docker image building and publishing

### [multi-language-ci.yml](multi-language-ci.yml)
Complete CI/CD pipeline for projects using multiple programming languages.

**Best for:** Full-stack applications with separate frontend and backend

**Includes:**
- Unified linting across all languages
- Security scanning for multiple languages
- Separate test jobs for different parts of the project
- Multiple Docker images for different components

## 🚀 How to Use

1. Choose the example that best matches your project
2. Copy the file to your repository as `.github/workflows/ci.yml`
3. Customize the inputs to match your project:
   - Language settings (`languages` input)
   - Node.js or Python version
   - Test commands
   - Docker image settings (if applicable)
4. Commit and push to see the workflow in action

## 🔧 Customization Guide

### Adjust Languages for CodeQL

Change the `languages` input based on your project:

```yaml
# Single language
languages: '["javascript"]'

# Multiple languages
languages: '["javascript", "python", "go"]'
```

Supported languages: `javascript`, `python`, `java`, `go`, `cpp`, `csharp`, `ruby`

### Change Test Commands

Update the `test-command` to match your project:

```yaml
# JavaScript examples
test-command: npm test
test-command: npm run test:coverage
test-command: yarn test

# Python examples
test-command: pytest
test-command: pytest --cov
test-command: python -m unittest discover
```

### Add Environment Variables

```yaml
jobs:
  test:
    uses: mudman1986/mudman-reusable/.github/workflows/reusable-test-suite.yml@main
    with:
      node-version: '20'
      test-command: npm test
    secrets:
      NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
```

### Skip Docker Building

Simply remove or comment out the `docker` job if you don't need container images.

### Run Only on Specific Branches

```yaml
on:
  push:
    branches: [ main, staging, production ]
  pull_request:
    branches: [ main ]
```

### Schedule Security Scans

```yaml
on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]
  schedule:
    - cron: '0 0 * * 1'  # Every Monday at midnight
```

## 📝 Tips

1. **Start simple**: Use `simple-ci.yml` first, then add more features
2. **Test incrementally**: Add one workflow at a time to troubleshoot easily
3. **Check Actions tab**: View workflow runs in your repository's Actions tab
4. **Read the logs**: Detailed logs help diagnose any issues
5. **Use branch protection**: Require workflows to pass before merging PRs

## 🔗 Related Documentation

- [Main README](../README.md) - Complete documentation
- [Quick Start Guide](../QUICK_START.md) - Step-by-step setup instructions
- [GitHub Actions Documentation](https://docs.github.com/en/actions)

## 💡 Need Help?

- Check the workflow file comments for guidance
- Review the main [README.md](../README.md) for detailed parameter descriptions
- Open an issue in this repository for support
