# Assignment 4: Professional Git Workflows

## Learning Objectives
- Understand the purpose and importance of version control
- Create atomic commits with meaningful messages
- Implement feature branching workflows
- Resolve merge conflicts
- Use Git effectively in a team environment
- Apply Git best practices to a Python project

## Background

Git, created by Linus Torvalds in 2005 for Linux kernel development, has become the industry standard for version control. This assignment will teach you professional Git workflows using our calculator project as an example.

## Prerequisites
- Git installed on your system
- Python 3.6+ installed
- Virtual environment setup (from Assignment 1)
- Basic Git knowledge (init, add, commit, push)

## Part 1: Historical Context and Setup

### Task 1.1: Understanding Version Control Evolution

Read about the evolution of version control systems:

1. **Local VCS**: Early systems like RCS (1982) that tracked changes on a single computer
2. **Centralized VCS**: Systems like CVS (1990) and SVN (2000) with a central server
3. **Distributed VCS**: Modern systems like Git (2005) and Mercurial where every developer has a full repository copy

**Reflection Questions:**
1. Why was distributed version control a revolutionary improvement over centralized systems?
2. How did Git's design goals (speed, distributed nature, non-linear development) reflect the needs of the Linux kernel project?

### Task 1.2: Setting Up the Calculator Project Repository

1. Create a new directory for the calculator project:
   ```bash
   mkdir python_calculator
   cd python_calculator
   ```

2. Initialize a Git repository:
   ```bash
   git init
   ```

3. Create a basic project structure:
   ```bash
   mkdir -p src/calculator tests docs
   touch README.md
   touch src/calculator/__init__.py
   touch src/calculator/operations.py
   touch tests/__init__.py
   touch tests/test_operations.py
   ```

4. Create a basic README.md file:
   ```markdown
   # Python Calculator

   A simple calculator implementation demonstrating professional Python development practices.

   ## Features
   - Basic arithmetic operations
   - Unit testing
   - Code quality standards
   - Professional Git workflow
   ```

5. Make your first commit:
   ```bash
   git add .
   git commit -m "Initial project structure"
   ```

## Part 2: Implementing Features with Atomic Commits

### Task 2.1: Create the Addition Operation

1. Create a feature branch:
   ```bash
   git checkout -b feature/addition
   ```

2. Implement the addition function in `src/calculator/operations.py`:
   ```python
   """Basic arithmetic operations for the calculator."""

   def add(a, b):
       """
       Add two numbers and return the result.
       
       Args:
           a: First number
           b: Second number
           
       Returns:
           The sum of a and b
       """
       return a + b
   ```

3. Create a test in `tests/test_operations.py`:
   ```python
   """Tests for calculator operations."""
   
   from calculator.operations import add

   def test_add():
       """Test the add function."""
       assert add(1, 2) == 3
       assert add(-1, 1) == 0
       assert add(0, 0) == 0
   ```

4. Make an atomic commit:
   ```bash
   git add src/calculator/operations.py
   git commit -m "feat: implement addition operation"
   
   git add tests/test_operations.py
   git commit -m "test: add tests for addition operation"
   ```

### Task 2.2: Create the Subtraction Operation

1. Create a new feature branch from main:
   ```bash
   git checkout main
   git checkout -b feature/subtraction
   ```

2. Implement the subtraction function in `operations.py` and its test in `test_operations.py`.

3. Make atomic commits for the implementation and tests.

## Part 3: Advanced Git Techniques

### Task 3.1: Creating Good Commit Messages

Commit messages should follow the conventional commits format:
```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

Types include:
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `test`: Adding or correcting tests
- `refactor`: Code change that neither fixes a bug nor adds a feature
- `style`: Formatting changes

For example:
```
feat(operations): implement multiplication function

Add multiplication operation to calculator operations module.
Includes input validation and documentation.

Closes #42
```

Update your previous commit messages using interactive rebase:
```bash
git rebase -i HEAD~2
```

### Task 3.2: Handling Merge Conflicts

1. Merge your feature branches to main:
   ```bash
   git checkout main
   git merge feature/addition
   git merge feature/subtraction
   ```

2. Create a new feature branch for division:
   ```bash
   git checkout -b feature/division
   ```

3. Implement division in `operations.py`:
   ```python
   def divide(a, b):
       """
       Divide a by b and return the result.
       
       Args:
           a: Dividend
           b: Divisor
           
       Returns:
           The quotient a/b
           
       Raises:
           ZeroDivisionError: If b is 0
       """
       if b == 0:
           raise ZeroDivisionError("Cannot divide by zero")
       return a / b
   ```

4. Create a conflicting multiplication feature branch:
   ```bash
   git checkout main
   git checkout -b feature/multiplication
   ```

5. Implement multiplication, but also modify the division function signature:
   ```python
   def multiply(a, b):
       """
       Multiply two numbers and return the result.
       
       Args:
           a: First factor
           b: Second factor
           
       Returns:
           The product of a and b
       """
       return a * b
       
   def divide(numerator, denominator):
       """Division operation with renamed parameters."""
       if denominator == 0:
           raise ZeroDivisionError("Cannot divide by zero")
       return numerator / denominator
   ```

6. Merge the multiplication branch first:
   ```bash
   git checkout main
   git merge feature/multiplication
   ```

7. Now try to merge the division branch and resolve the conflict:
   ```bash
   git merge feature/division
   ```

8. Open the conflicted file and resolve the conflict, keeping both the division and multiplication functions with consistent parameter naming.

9. Complete the merge:
   ```bash
   git add src/calculator/operations.py
   git commit -m "merge: combine division and multiplication features"
   ```

## Part 4: Collaborative Workflows

### Task 4.1: Setting Up a Remote Repository

1. Create a new repository on GitHub
2. Add the remote to your local repository:
   ```bash
   git remote add origin https://github.com/yourusername/python_calculator.git
   ```
3. Push your main branch:
   ```bash
   git push -u origin main
   ```

### Task 4.2: Simulating Collaborative Development

In this task, you'll simulate working with a teammate by using two different directories.

1. Clone your repository to a different directory (simulating a teammate):
   ```bash
   cd ..
   git clone https://github.com/yourusername/python_calculator.git calculator_teammate
   cd calculator_teammate
   ```

2. In the original directory, create a new feature:
   ```bash
   cd ../python_calculator
   git checkout -b feature/square-root
   ```

3. Add a square root function to `operations.py` and push the branch:
   ```bash
   git push -u origin feature/square-root
   ```

4. In the "teammate" directory, create a different feature:
   ```bash
   cd ../calculator_teammate
   git checkout -b feature/power
   ```

5. Add a power function to `operations.py` and push the branch:
   ```bash
   git push -u origin feature/power
   ```

6. Create pull requests on GitHub for both features

7. Review each other's code (in this case, you review both as you're simulating both roles)

8. Merge the pull requests on GitHub

9. Pull the changes to your local repositories:
   ```bash
   git checkout main
   git pull
   ```

## Part 5: Professional Git Practices

### Task 5.1: Creating an Effective .gitignore

1. Create a comprehensive .gitignore file for Python projects:
   ```bash
   # Common Python ignores
   __pycache__/
   *.py[cod]
   *$py.class
   *.so
   .Python
   env/
   venv/
   ENV/
   build/
   develop-eggs/
   dist/
   downloads/
   eggs/
   .eggs/
   lib/
   lib64/
   parts/
   sdist/
   var/
   *.egg-info/
   .installed.cfg
   *.egg
   
   # Unit test / coverage reports
   htmlcov/
   .tox/
   .coverage
   .coverage.*
   .cache
   nosetests.xml
   coverage.xml
   *.cover
   .hypothesis/
   
   # Jupyter Notebook
   .ipynb_checkpoints
   
   # VS Code
   .vscode/*
   !.vscode/settings.json.example
   
   # PyCharm
   .idea/
   
   # OS specific
   .DS_Store
   .DS_Store?
   ._*
   .Spotlight-V100
   .Trashes
   ehthumbs.db
   Thumbs.db
   ```

2. Commit this file:
   ```bash
   git add .gitignore
   git commit -m "chore: add comprehensive .gitignore file"
   ```

### Task 5.2: Using Git Hooks

1. Create a pre-commit hook that runs tests and linting:

   Create a file at `.git/hooks/pre-commit`:
   ```bash
   #!/bin/bash
   
   # Activate virtual environment
   source venv/bin/activate
   
   # Run pylint
   pylint src tests
   if [ $? -ne 0 ]; then
       echo "Linting failed, commit aborted"
       exit 1
   fi
   
   # Run tests
   pytest
   if [ $? -ne 0 ]; then
       echo "Tests failed, commit aborted"
       exit 1
   fi
   
   exit 0
   ```

2. Make the hook executable:
   ```bash
   chmod +x .git/hooks/pre-commit
   ```

3. Try making a commit with a failing test to see the hook in action.

## Part 6: Git for Project Management

### Task 6.1: Using GitHub Issues

1. Create several issues on GitHub for your calculator project:
   - Add a memory function
   - Create a command-line interface
   - Fix division by zero error message
   - Add scientific notation support

2. Label and prioritize the issues

### Task 6.2: Using Branches and PRs for Issue Management

1. Pick one of the issues
2. Create a branch with the issue number:
   ```bash
   git checkout -b feature/42-memory-function
   ```
3. Implement the feature
4. Create a PR that references the issue:
   ```
   feat: add calculator memory function
   
   Implements a memory storage and recall function for the calculator.
   
   Closes #42
   ```

## Submission Requirements

Submit a link to your GitHub repository containing:

1. The completed calculator project with:
   - At least 5 operations (add, subtract, multiply, divide, and one more)
   - Tests for all operations
   - Proper documentation
   - Meaningful commit history following conventional commits format

2. A reflection document addressing:
   - How did atomic commits improve your development process?
   - What challenges did you face with merge conflicts and how did you resolve them?
   - How would your Git workflow change when working in a team vs. solo?
   - How does Git support or enhance software quality?

## Additional Resources

- [Pro Git Book](https://git-scm.com/book/en/v2) - Comprehensive Git guide
- [Conventional Commits](https://www.conventionalcommits.org/) - Specification for commit messages
- [GitHub Flow](https://guides.github.com/introduction/flow/) - Simple workflow using GitHub
- [Git Cheat Sheet](https://education.github.com/git-cheat-sheet-education.pdf) - Quick reference
- [Learn Git Branching](https://learngitbranching.js.org/) - Interactive Git visualization tool
