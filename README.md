# Student Python Development Guide

## Overview
A comprehensive guide designed for college students learning modern Python development tools and best practices. This repository serves as both a reference and a hands-on learning resource to help you build professional software engineering skills.

## Documentation

This repository contains detailed documentation organized into the following sections:

- [📚 Project Documentation Home](docs/README.md) - Start here for an overview of all documentation
- [🚀 Project Setup](docs/01-project-setup.md) - Step-by-step instructions for setting up Python virtual environments and managing dependencies
- [⚙️ Development Workflow](docs/02-development-workflow.md) - Best practices for writing, organizing, and maintaining Python code
- [🧪 Testing with pytest](docs/03-testing.md) - How to write effective tests and implement test-driven development
- [✅ Code Quality with pylint](docs/04-code-quality.md) - Tools and techniques to ensure your code follows Python standards
- [🔄 Automation with GitHub Actions](docs/05-github-actions.md) - Setting up continuous integration pipelines for your projects
- [💻 VS Code Integration](docs/06-vscode-setup.md) - Setting up VS Code for Python development across Windows, macOS, and Linux
- [📝 Git Best Practices](docs/07-git-best-practices.md) - Professional Git workflows including atomic commits and branching strategies

## The Python Calculator Project

Throughout the assignments in this guide, you'll build a professional-grade calculator application in Python. While the calculator itself is simple, you'll apply industry-standard practices to its development:

- Setting up proper development environments
- Implementing test-driven development
- Ensuring code quality through linting
- Using professional Git workflows
- Implementing continuous integration

This approach mirrors real-world software development, where even simple applications benefit from professional practices. The simplicity of the calculator allows you to focus on mastering the tools and workflows rather than complex application logic.

## Progressive Learning Path

The assignments in this guide follow a deliberate progression that mirrors professional development workflows:

1. **Environment Setup** → Create the foundation for development with virtual environments and package management
2. **Code Quality** → Implement quality standards before expanding functionality
3. **Testing** → Ensure reliability through automated tests
4. **Version Control** → Manage changes professionally and collaborate effectively

Each assignment builds directly on the previous one, with the calculator project growing more robust at each step.

## Hands-on Assignments

The best way to learn is by doing! Complete these practical assignments to build real-world skills:

- [**Assignment 1: Virtual Environments and Pip**](assignments/01-virtual-env-pip.md)  
  Set up your development environment and create the basic calculator structure with addition and subtraction operations.

- [**Assignment 2: Code Quality with Pylint**](assignments/02-pylint-code-quality.md)  
  Apply code quality standards to your existing calculator code and improve its structure and documentation.

- [**Assignment 3: Testing with Pytest**](assignments/03-pytest.md)  
  Add comprehensive tests for your calculator and use test-driven development to implement multiplication and division features.

- [**Assignment 4: Professional Git Workflows**](assignments/04-git-workflows.md)  
  Use Git branching strategies to implement scientific calculator features (square root, power, etc.) and manage the project professionally.

Each assignment includes detailed instructions, example code, and reflection questions to deepen your understanding.

## Who This Guide Is For

This guide is ideal for:
- College students studying computer science or software engineering
- Self-taught programmers looking to adopt professional practices
- Junior developers wanting to improve their development workflow
- Anyone transitioning from casual Python coding to professional development

No advanced Python knowledge is required, but basic familiarity with Python syntax is recommended.

## Platform Support

This guide provides instructions for:
- **Windows** with WSL2 (Windows Subsystem for Linux)
- **macOS** using Terminal
- **Linux** distributions (Ubuntu, Debian, etc.)

## Getting Started

Follow these steps to begin your learning journey:

```bash
# Clone the repository
git clone https://github.com/kaw393939/student_python_dev_guide.git

# Navigate to the project directory
cd student_python_dev_guide

# Set up virtual environment
python -m venv venv

# Activate the virtual environment
# On Windows with WSL2 or Linux:
source venv/bin/activate
# On Windows Command Prompt:
venv\Scripts\activate
# On Windows PowerShell:
.\venv\Scripts\Activate.ps1
# On macOS:
source venv/bin/activate

# Install dependencies (if applicable)
pip install -r requirements.txt
```

For detailed setup instructions, see our [Project Setup Guide](docs/01-project-setup.md) and [VS Code Integration Guide](docs/06-vscode-setup.md).

## Purpose

This guide is designed for college students aiming to develop professional software engineering skills with Python. You'll learn:

- **Virtual Environments**: Create isolated development environments to avoid dependency conflicts
- **Package Management**: Use pip effectively to manage project dependencies
- **Testing**: Write automated tests to verify your code works correctly
- **Code Quality**: Follow industry standards for clean, maintainable code
- **CI/CD**: Implement continuous integration workflows using GitHub Actions
- **Version Control**: Master Git workflows and collaborative development

These skills are essential for internships, jobs, and personal projects, giving you a competitive edge in the software development industry.

## Job Market Relevance

The practices taught in this guide are specifically chosen to address common requirements in software engineering job postings:

- **78%** of Python job listings mention version control (typically Git)
- **65%** require experience with testing frameworks
- **52%** specify CI/CD knowledge
- **47%** mention code quality tools or practices

By mastering these skills, you'll be able to demonstrate professional-level competencies that employers actively seek.

## Historical Context

This guide teaches tools and practices that evolved over decades of software development:

- **Python** was created by Guido van Rossum in 1991, with Python 3 releasing in 2008
- **Version control** evolved from RCS (1982) to CVS (1990) to SVN (2000) to Git (2005)
- **Test-driven development** emerged in the late 1990s as part of Extreme Programming
- **Continuous integration** concepts date back to the 1990s but became mainstream in the 2010s
- **Code linting** tools have roots in the 1978 Unix "lint" program for C

Understanding this history helps appreciate why these practices developed and why they're valuable.

## Contributing

Found an issue or have a suggestion? Contributions are welcome!

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/improvement`)
3. Make your changes
4. Commit with clear messages (`git commit -m 'Add new section on...'`)
5. Push to your branch (`git push origin feature/improvement`)
6. Open a Pull Request

## License
MIT License
