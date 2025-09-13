# Python Development Learning Roadmap

This visual guide illustrates your learning journey through professional Python development practices.

```
┌───────────────────────┐                ┌───────────────────────┐
│                       │                │                       │
│  ASSIGNMENT 1:        │                │  ASSIGNMENT 2:        │
│  VIRTUAL ENVIRONMENTS │                │  CODE QUALITY         │
│  & PACKAGE MANAGEMENT ├───────────────►│  WITH PYLINT          │
│                       │                │                       │
│  • Set up venv        │                │  • Learn PEP 8        │
│  • Manage packages    │                │  • Configure pylint   │
│  • Create calculator  │                │  • Improve code       │
│  • Basic operations   │                │  • Add Calculator     │
│                       │                │  • class              │
└───────────┬───────────┘                └───────────┬───────────┘
            │                                        │
            │ Foundation                             │ Quality
            ▼                                        ▼
┌───────────────────────┐                ┌───────────────────────┐
│                       │                │                       │
│  ASSIGNMENT 3:        │                │  ASSIGNMENT 4:        │
│  TESTING WITH         │                │  PROFESSIONAL         │
│  PYTEST               │◄───────────────┤  GIT WORKFLOWS        │
│                       │                │                       │
│  • Write tests        │                │  • Atomic commits     │
│  • Test coverage      │                │  • Branching          │
│  • TDD for new        │                │  • Merge conflicts    │
│  • features           │                │  • GitHub PRs         │
│                       │                │                       │
└───────────────────────┘                └───────────────────────┘
            ▲                                        ▲
            │ Reliability                            │ Collaboration
            │                                        │
            └────────────────────────────────────────┘

```

## Skills Progression

This table shows how your skills will develop through each assignment:

| Skill Area          | Assignment 1 | Assignment 2 | Assignment 3 | Assignment 4 |
|---------------------|:------------:|:------------:|:------------:|:------------:|
| Environment Setup   | ★★★         | ★★★★        | ★★★★        | ★★★★★       |
| Package Management  | ★★★         | ★★★★        | ★★★★        | ★★★★★       |
| Code Organization   | ★★          | ★★★★        | ★★★★        | ★★★★★       |
| Documentation       | ★           | ★★★★        | ★★★★        | ★★★★★       |
| Testing             | ★           | ★★          | ★★★★★       | ★★★★★       |
| Error Handling      | ★           | ★★          | ★★★★        | ★★★★★       |
| Version Control     | ★           | ★★          | ★★★         | ★★★★★       |
| Collaboration       | ★           | ★           | ★★          | ★★★★★       |

## Calculator Project Evolution

Watch how your calculator project evolves through the assignments:

1. **Assignment 1**: Basic calculator with addition and subtraction functions
   ```python
   def add(a, b): return a + b
   def subtract(a, b): return a - b
   ```

2. **Assignment 2**: Well-documented calculator class with improved code quality
   ```python
   class Calculator:
       def __init__(self): self.memory = 0
       def add(self, a, b): return add(a, b)
       def subtract(self, a, b): return subtract(a, b)
   ```

3. **Assignment 3**: Calculator with tested multiplication, division, and scientific operations
   ```python
   # New operations added using TDD
   def multiply(a, b): return a * b
   def divide(a, b): return a / b
   def square_root(x): return math.sqrt(x)
   ```

4. **Assignment 4**: Feature-complete calculator with memory functions and display
   ```python
   # New features in separate branches
   def memory_add(self, value): self.memory += value
   def sin(x): return math.sin(x)
   def update_display(self, value): self.display = str(value)
   ```

## Professional Skills Application

Each assignment teaches you professional skills applicable to any software project:

- **Assignment 1**: Environment isolation and dependency management
- **Assignment 2**: Code quality standards and maintainability practices
- **Assignment 3**: Test-driven development and code reliability
- **Assignment 4**: Professional collaboration and version control

By completing this learning path, you'll have practical experience with the foundational skills required for professional Python development.
