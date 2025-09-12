# Code Quality with pylint

This guide explains how to use pylint to maintain high code quality.

## Setting Up pylint

Ensure pylint is installed in your virtual environment:

```bash
pip install pylint
```

## Running pylint

```bash
# Check a single file
pylint filename.py

# Check multiple files
pylint file1.py file2.py

# Check all Python files in a directory
pylint your_package_name/
```

## Understanding pylint Output

pylint generates a report with:
- A score out of 10
- Messages categorized by type (error, warning, convention, refactor)
- Line numbers and descriptions of issues

Example output:
```
************* Module example
example.py:10:0: C0303: Trailing whitespace (trailing-whitespace)
example.py:15:0: C0111: Missing function docstring (missing-docstring)
```

## Common pylint Messages

- **C0111**: Missing docstring
- **C0103**: Invalid name (doesn't conform to naming convention)
- **W0612**: Unused variable
- **E1101**: Instance has no 'attribute' member
- **R0201**: Method could be a function
- **R0903**: Too few public methods

## Configuring pylint

Create a `.pylintrc` file to customize pylint behavior:

```bash
# Generate a default configuration file
pylint --generate-rcfile > .pylintrc
```

Common configurations:
```ini
[MASTER]
# Maximum line length
max-line-length=100

[MESSAGES CONTROL]
# Disable specific checks
disable=C0111,C0103

[REPORTS]
# Output format
output-format=text
```

## Integrating with Your Editor

Most code editors support pylint integration:

- **VS Code**: Install the Python extension
- **PyCharm**: Enable pylint in settings
- **Sublime Text**: Install SublimeLinter-pylint

## Automatically Fixing Issues

Some issues can be fixed automatically using tools like `autopep8` or `black`:

```bash
pip install autopep8
autopep8 --in-place --aggressive --aggressive filename.py
```

## Next Steps

Learn how to automate testing and linting with [GitHub Actions](05-github-actions.md).
