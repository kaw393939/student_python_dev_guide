# Project Setup

This guide will walk you through setting up your Python development environment.

## Setting Up a Virtual Environment

Python virtual environments keep your project dependencies isolated from other projects.

### Creating a Virtual Environment

```bash
# Navigate to your project directory
cd your-project-directory

# Create a virtual environment
python -m venv venv

# Activate the virtual environment
# On Windows:
venv\Scripts\activate
# On macOS/Linux:
source venv/bin/activate
```

When the virtual environment is active, you'll see `(venv)` in your command prompt.

## Installing Dependencies

```bash
# Install project dependencies
pip install -r requirements.txt

# If you don't have a requirements.txt, you can install packages individually:
pip install pytest pylint
```

## Creating a requirements.txt File

```bash
# Generate requirements.txt from your current environment
pip freeze > requirements.txt
```

## Deactivating the Virtual Environment

When you're done working on your project:

```bash
deactivate
```

## Git Configuration

If you haven't set up Git yet:

```bash
# Set your name and email
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Initialize a new Git repository (if needed)
git init

# Create a .gitignore file
touch .gitignore
```

Example content for `.gitignore`:

```
# Virtual Environment
venv/
env/
.env/

# Python cache files
__pycache__/
*.py[cod]
*$py.class
.pytest_cache/

# Distribution / packaging
dist/
build/
*.egg-info/

# Coverage reports
htmlcov/
.coverage
.coverage.*
coverage.xml
*.cover

# VS Code
.vscode/

# PyCharm
.idea/

# Jupyter Notebook
.ipynb_checkpoints
```

## Next Steps

Now that your environment is set up, check out the [Development Workflow](02-development-workflow.md) guide.
