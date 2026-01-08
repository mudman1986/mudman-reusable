# Contributing to mudman-reusable

Thank you for your interest in contributing to this repository! This guide will help you add new reusable workflows and composite actions.

## 🎯 What to Contribute

We welcome contributions of:
- **New reusable workflows** for common CI/CD tasks
- **Composite actions** that simplify common setup steps
- **Documentation improvements**
- **Example workflows** for different technology stacks
- **Bug fixes** in existing workflows

## 📝 Guidelines

### Reusable Workflows

When creating a new reusable workflow:

1. **Place it in** `.github/workflows/`
2. **Name it** `reusable-<descriptive-name>.yml`
3. **Use** `workflow_call` trigger
4. **Include:**
   - Clear description in the `name` field
   - Well-documented `inputs` with descriptions and defaults
   - Appropriate `permissions` (use least privilege principle)
   - Error handling where applicable

**Example structure:**

```yaml
name: Descriptive Workflow Name

on:
  workflow_call:
    inputs:
      input-name:
        description: 'Clear description of what this input does'
        required: false
        type: string
        default: 'sensible-default'
    secrets:
      secret-name:
        description: 'Description of the secret'
        required: false

jobs:
  job-name:
    name: Human Readable Job Name
    runs-on: ubuntu-latest
    
    permissions:
      contents: read
      # Add other permissions as needed
    
    steps:
      - name: Descriptive Step Name
        uses: actions/checkout@v4
      
      # More steps...
```

### Composite Actions

When creating a new composite action:

1. **Create a directory** `.github/actions/<action-name>/`
2. **Create** `action.yml` in that directory
3. **Include:**
   - `name`, `description`, and `author` fields
   - Well-documented inputs with descriptions
   - Use `composite` as the runs type
   - Clear step names

**Example structure:**

```yaml
name: 'Action Name'
description: 'Clear description of what this action does'
author: 'mudman1986'

inputs:
  input-name:
    description: 'Description of the input'
    required: false
    default: 'default-value'

runs:
  using: 'composite'
  steps:
    - name: Step Name
      uses: some/action@v1
      with:
        param: ${{ inputs.input-name }}
```

### Documentation

All new workflows and actions must include:

1. **Documentation in README.md** with:
   - Clear description
   - Usage example
   - Input/output table
   - Any prerequisites

2. **Example usage** in the `examples/` directory if applicable

3. **Inline comments** for complex logic

## 🔍 Testing Your Contribution

Before submitting:

1. **Validate YAML syntax:**
   ```bash
   python3 -c "import yaml; yaml.safe_load(open('your-file.yml'))"
   ```

2. **Test in a real repository** by:
   - Pushing your branch to a fork
   - Creating a test repository that uses your workflow
   - Verifying it works as expected

3. **Check for security issues:**
   - Don't expose secrets
   - Use specific action versions (not `@main` in action uses)
   - Follow the principle of least privilege for permissions

## 📋 Pull Request Process

1. **Fork the repository** and create a new branch
2. **Make your changes** following the guidelines above
3. **Update documentation:**
   - Add your workflow/action to README.md
   - Create example usage if applicable
   - Update QUICK_START.md if relevant
4. **Commit with clear messages:**
   ```
   Add reusable workflow for XYZ
   
   - Created .github/workflows/reusable-xyz.yml
   - Added documentation and examples
   - Tested with sample project
   ```
5. **Create a pull request** with:
   - Clear title describing the change
   - Description of what the workflow/action does
   - Link to any test runs showing it works

## ✅ Checklist

Before submitting your PR, ensure:

- [ ] YAML syntax is valid
- [ ] All inputs have descriptions and sensible defaults
- [ ] Permissions are minimal and appropriate
- [ ] Documentation is added to README.md
- [ ] Example usage is provided
- [ ] You've tested the workflow in a real repository
- [ ] No secrets or sensitive data are exposed
- [ ] Code follows existing patterns in the repository

## 🎨 Code Style

- Use **2 spaces** for indentation in YAML
- Use **kebab-case** for input names (e.g., `my-input-name`)
- Use **descriptive names** for jobs and steps
- Include **comments** for non-obvious logic
- Keep **inputs focused** - one responsibility per workflow

## 🔒 Security

- **Never hardcode secrets** in workflow files
- **Pin action versions** to specific tags or SHAs when possible
- **Use minimal permissions** - only request what's needed
- **Validate inputs** where applicable
- **Review dependencies** for known vulnerabilities

## 🌟 Best Practices

1. **Reusability**: Make workflows flexible but not overly complex
2. **Documentation**: Over-document rather than under-document
3. **Defaults**: Provide sensible defaults for all optional inputs
4. **Versioning**: Use semantic versioning when suggesting version pins
5. **Compatibility**: Consider backwards compatibility when making changes

## 💡 Ideas for Contributions

Looking for ideas? Consider adding:

- Workflows for specific frameworks (Next.js, Django, etc.)
- Language-specific linting workflows
- Deployment workflows (AWS, Azure, GCP)
- Release automation workflows
- Performance testing workflows
- Accessibility testing workflows
- Infrastructure as Code validation (Terraform, CloudFormation)

## 📞 Questions?

If you have questions about contributing:
1. Check existing workflows for examples
2. Review the GitHub Actions documentation
3. Open an issue to discuss your idea before implementing

## 📜 License

By contributing, you agree that your contributions will be licensed under the same license as this project (MIT License).

Thank you for contributing! 🎉
