````markdown
# Assignment 7: Python Packaging and Distribution

## Learning Objectives
- Master modern Python packaging with pyproject.toml
- Understand semantic versioning and dependency management
- Publish packages to PyPI and PyPI test environments
- Create distributable applications with entry points
- Implement proper package structure and metadata
- Apply packaging security and best practices

## Background

Python packaging has evolved significantly with modern tools like pyproject.toml, Poetry, and improved pip. This assignment teaches you to create professional, distributable Python packages that others can easily install and use.

### Modern Packaging Standards

This assignment follows PEP 517/518 standards for build system requirements and uses:
- **pyproject.toml**: Modern configuration format replacing setup.py
- **Semantic Versioning**: Version numbering that conveys meaning
- **Entry Points**: Creating command-line tools from your packages
- **Build Backends**: Using setuptools, poetry, or other build systems

Professional Python developers must know how to package and distribute their code effectively.

## Prerequisites
- Completed Assignments 1-6
- Calculator project with configuration management
- Understanding of virtual environments
- Basic knowledge of command-line interfaces

## Time to Complete
Expect to spend approximately 3-4 hours on this assignment.

## Part 1: Modern Package Structure

### Task 1.1: Restructure Project for Distribution

1. Create the modern package structure:
   ```
   calculator-package/
   ├── pyproject.toml
   ├── README.md
   ├── LICENSE
   ├── CHANGELOG.md
   ├── MANIFEST.in
   ├── src/
   │   └── calculator/
   │       ├── __init__.py
   │       ├── __main__.py
   │       ├── cli.py
   │       ├── calculator.py
   │       ├── operations.py
   │       ├── scientific.py
   │       ├── config.py
   │       ├── logging_config.py
   │       └── version.py
   ├── tests/
   ├── docs/
   ├── config/
   └── scripts/
   ```

2. Create `src/calculator/__init__.py`:
   ```python
   """
   Calculator Package
   
   A professional calculator library with support for basic and scientific operations,
   configuration management, and comprehensive logging.
   """
   from .calculator import Calculator
   from .operations import add, subtract, multiply, divide
   from .scientific import (
       square_root, power, sin, cos, tan, log, natural_log,
       factorial, degrees_to_radians, radians_to_degrees
   )
   from .config import config
   from .version import __version__
   
   __all__ = [
       'Calculator',
       'add', 'subtract', 'multiply', 'divide',
       'square_root', 'power', 'sin', 'cos', 'tan',
       'log', 'natural_log', 'factorial',
       'degrees_to_radians', 'radians_to_degrees',
       'config', '__version__'
   ]
   
   # Package metadata
   __author__ = "Your Name"
   __email__ = "your.email@example.com"
   __description__ = "A professional calculator library with advanced features"
   ```

3. Create `src/calculator/version.py`:
   ```python
   """Version information for the calculator package."""
   
   __version__ = "1.0.0"
   __version_info__ = tuple(map(int, __version__.split('.')))
   
   
   def get_version():
       """Return the version string."""
       return __version__
   
   
   def get_version_info():
       """Return the version as a tuple of integers."""
       return __version_info__
   ```

4. Create `src/calculator/__main__.py`:
   ```python
   """Main module for running calculator as a package."""
   from .cli import main
   
   if __name__ == '__main__':
       main()
   ```

### Task 1.2: Create Command-Line Interface

1. Create `src/calculator/cli.py`:
   ```python
   """Command-line interface for the calculator."""
   import sys
   import argparse
   from typing import List, Optional
   
   from .calculator import Calculator
   from .config import config
   from .logging_config import setup_logging
   from .version import __version__
   
   
   def create_parser() -> argparse.ArgumentParser:
       """Create and configure the argument parser."""
       parser = argparse.ArgumentParser(
           prog='calculator',
           description='A professional calculator with basic and scientific operations',
           epilog='Examples:\n'
                  '  calculator add 5 3\n'
                  '  calculator --interactive\n'
                  '  calculator --version',
           formatter_class=argparse.RawDescriptionHelpFormatter
       )
       
       # Version
       parser.add_argument(
           '--version', action='version',
           version=f'calculator {__version__}'
       )
       
       # Interactive mode
       parser.add_argument(
           '-i', '--interactive',
           action='store_true',
           help='Start interactive calculator mode'
       )
       
       # Configuration options
       parser.add_argument(
           '--precision',
           type=int,
           default=None,
           help='Number of decimal places for results'
       )
       
       parser.add_argument(
           '--log-level',
           choices=['DEBUG', 'INFO', 'WARNING', 'ERROR', 'CRITICAL'],
           default='INFO',
           help='Set logging level'
       )
       
       # Operation subcommands
       subparsers = parser.add_subparsers(
           dest='operation',
           help='Available operations'
       )
       
       # Basic operations
       add_parser = subparsers.add_parser('add', help='Add two numbers')
       add_parser.add_argument('a', type=float, help='First number')
       add_parser.add_argument('b', type=float, help='Second number')
       
       subtract_parser = subparsers.add_parser('subtract', help='Subtract two numbers')
       subtract_parser.add_argument('a', type=float, help='First number')
       subtract_parser.add_argument('b', type=float, help='Second number')
       
       multiply_parser = subparsers.add_parser('multiply', help='Multiply two numbers')
       multiply_parser.add_argument('a', type=float, help='First number')
       multiply_parser.add_argument('b', type=float, help='Second number')
       
       divide_parser = subparsers.add_parser('divide', help='Divide two numbers')
       divide_parser.add_argument('a', type=float, help='First number')
       divide_parser.add_argument('b', type=float, help='Second number')
       
       # Scientific operations
       sqrt_parser = subparsers.add_parser('sqrt', help='Square root of a number')
       sqrt_parser.add_argument('x', type=float, help='Number')
       
       power_parser = subparsers.add_parser('power', help='Raise number to power')
       power_parser.add_argument('base', type=float, help='Base number')
       power_parser.add_argument('exponent', type=float, help='Exponent')
       
       sin_parser = subparsers.add_parser('sin', help='Sine of angle in radians')
       sin_parser.add_argument('x', type=float, help='Angle in radians')
       
       cos_parser = subparsers.add_parser('cos', help='Cosine of angle in radians')
       cos_parser.add_argument('x', type=float, help='Angle in radians')
       
       tan_parser = subparsers.add_parser('tan', help='Tangent of angle in radians')
       tan_parser.add_argument('x', type=float, help='Angle in radians')
       
       return parser
   
   
   def interactive_mode(calc: Calculator) -> None:
       """Run calculator in interactive mode."""
       print(f"Calculator v{__version__} - Interactive Mode")
       print("Type 'help' for commands, 'quit' to exit")
       print("-" * 40)
       
       while True:
           try:
               user_input = input("calc> ").strip()
               
               if user_input.lower() in ['quit', 'exit', 'q']:
                   print("Goodbye!")
                   break
               
               if user_input.lower() == 'help':
                   print_help()
                   continue
               
               if user_input.lower() == 'history':
                   print_history(calc)
                   continue
               
               if user_input.lower() == 'clear':
                   calc.clear_history()
                   print("History cleared")
                   continue
               
               if user_input.lower() == 'stats':
                   print_stats(calc)
                   continue
               
               # Parse and execute command
               result = parse_interactive_command(calc, user_input)
               if result is not None:
                   print(f"Result: {result}")
           
           except KeyboardInterrupt:
               print("\nGoodbye!")
               break
           except Exception as e:
               print(f"Error: {e}")
   
   
   def print_help():
       """Print interactive mode help."""
       help_text = """
   Available commands:
   
   Basic Operations:
     add <a> <b>        - Add two numbers
     subtract <a> <b>   - Subtract b from a
     multiply <a> <b>   - Multiply two numbers
     divide <a> <b>     - Divide a by b
   
   Scientific Operations:
     sqrt <x>           - Square root of x
     power <base> <exp> - Raise base to power of exp
     sin <x>            - Sine of x (radians)
     cos <x>            - Cosine of x (radians)
     tan <x>            - Tangent of x (radians)
   
   Memory Operations:
     store <value>      - Store value in memory
     recall             - Recall value from memory
     clear_memory       - Clear memory
   
   Utility Commands:
     history            - Show calculation history
     clear              - Clear history
     stats              - Show calculator statistics
     help               - Show this help
     quit               - Exit calculator
   """
       print(help_text)
   
   
   def parse_interactive_command(calc: Calculator, command: str) -> Optional[float]:
       """Parse and execute an interactive command."""
       parts = command.split()
       if not parts:
           return None
       
       cmd = parts[0].lower()
       
       try:
           if cmd == 'add' and len(parts) == 3:
               return calc.add(float(parts[1]), float(parts[2]))
           elif cmd == 'subtract' and len(parts) == 3:
               return calc.subtract(float(parts[1]), float(parts[2]))
           elif cmd == 'multiply' and len(parts) == 3:
               return calc.multiply(float(parts[1]), float(parts[2]))
           elif cmd == 'divide' and len(parts) == 3:
               return calc.divide(float(parts[1]), float(parts[2]))
           elif cmd == 'sqrt' and len(parts) == 2:
               return calc.square_root(float(parts[1]))
           elif cmd == 'power' and len(parts) == 3:
               return calc.power(float(parts[1]), float(parts[2]))
           elif cmd == 'sin' and len(parts) == 2:
               from .scientific import sin
               return sin(float(parts[1]))
           elif cmd == 'cos' and len(parts) == 2:
               from .scientific import cos
               return cos(float(parts[1]))
           elif cmd == 'tan' and len(parts) == 2:
               from .scientific import tan
               return tan(float(parts[1]))
           elif cmd == 'store' and len(parts) == 2:
               calc.memory_store(float(parts[1]))
               print(f"Stored {parts[1]} in memory")
               return None
           elif cmd == 'recall' and len(parts) == 1:
               return calc.memory_recall()
           elif cmd == 'clear_memory' and len(parts) == 1:
               calc.memory_clear()
               print("Memory cleared")
               return None
           else:
               print(f"Unknown command or wrong number of arguments: {command}")
               print("Type 'help' for available commands")
               return None
       except ValueError as e:
           print(f"Invalid number format: {e}")
           return None
   
   
   def print_history(calc: Calculator):
       """Print calculation history."""
       history = calc.get_history()
       if not history:
           print("No history available")
           return
       
       print("Calculation History:")
       for i, entry in enumerate(history, 1):
           print(f"  {i}. {entry}")
   
   
   def print_stats(calc: Calculator):
       """Print calculator statistics."""
       stats = calc.get_stats()
       print("Calculator Statistics:")
       for key, value in stats.items():
           print(f"  {key}: {value}")
   
   
   def main(args: Optional[List[str]] = None) -> int:
       """Main entry point for the CLI."""
       parser = create_parser()
       parsed_args = parser.parse_args(args)
       
       # Setup logging
       setup_logging(level=parsed_args.log_level)
       
       # Override precision if specified
       if parsed_args.precision is not None:
           config.calculator.precision = parsed_args.precision
       
       # Initialize calculator
       calc = Calculator()
       
       # Interactive mode
       if parsed_args.interactive:
           interactive_mode(calc)
           return 0
       
       # Operation mode
       if not parsed_args.operation:
           parser.print_help()
           return 1
       
       try:
           # Execute the requested operation
           if parsed_args.operation == 'add':
               result = calc.add(parsed_args.a, parsed_args.b)
           elif parsed_args.operation == 'subtract':
               result = calc.subtract(parsed_args.a, parsed_args.b)
           elif parsed_args.operation == 'multiply':
               result = calc.multiply(parsed_args.a, parsed_args.b)
           elif parsed_args.operation == 'divide':
               result = calc.divide(parsed_args.a, parsed_args.b)
           elif parsed_args.operation == 'sqrt':
               result = calc.square_root(parsed_args.x)
           elif parsed_args.operation == 'power':
               result = calc.power(parsed_args.base, parsed_args.exponent)
           elif parsed_args.operation == 'sin':
               from .scientific import sin
               result = sin(parsed_args.x)
           elif parsed_args.operation == 'cos':
               from .scientific import cos
               result = cos(parsed_args.x)
           elif parsed_args.operation == 'tan':
               from .scientific import tan
               result = tan(parsed_args.x)
           else:
               print(f"Unknown operation: {parsed_args.operation}")
               return 1
           
           print(result)
           return 0
           
       except Exception as e:
           print(f"Error: {e}")
           return 1
   
   
   if __name__ == '__main__':
       sys.exit(main())
   ```

## Part 2: Creating pyproject.toml

### Task 2.1: Define Package Metadata

1. Create `pyproject.toml`:
   ```toml
   [build-system]
   requires = ["setuptools>=61.0", "wheel"]
   build-backend = "setuptools.build_meta"
   
   [project]
   name = "calculator-pro"
   version = "1.0.0"
   description = "A professional calculator library with advanced features"
   readme = "README.md"
   license = {file = "LICENSE"}
   authors = [
       {name = "Your Name", email = "your.email@example.com"}
   ]
   maintainers = [
       {name = "Your Name", email = "your.email@example.com"}
   ]
   keywords = ["calculator", "math", "scientific", "computation"]
   classifiers = [
       "Development Status :: 5 - Production/Stable",
       "Intended Audience :: Developers",
       "Intended Audience :: Education",
       "License :: OSI Approved :: MIT License",
       "Operating System :: OS Independent",
       "Programming Language :: Python :: 3",
       "Programming Language :: Python :: 3.8",
       "Programming Language :: Python :: 3.9",
       "Programming Language :: Python :: 3.10",
       "Programming Language :: Python :: 3.11",
       "Programming Language :: Python :: 3.12",
       "Topic :: Scientific/Engineering :: Mathematics",
       "Topic :: Software Development :: Libraries :: Python Modules",
       "Topic :: Utilities",
   ]
   requires-python = ">=3.8"
   dependencies = [
       "pydantic[dotenv]>=1.10.0",
       "cryptography>=3.4.0",
   ]
   
   [project.optional-dependencies]
   dev = [
       "pytest>=7.0.0",
       "pytest-cov>=4.0.0",
       "pytest-benchmark>=4.0.0",
       "black>=22.0.0",
       "pylint>=2.15.0",
       "mypy>=1.0.0",
       "pre-commit>=3.0.0",
       "tox>=4.0.0",
   ]
   test = [
       "pytest>=7.0.0",
       "pytest-cov>=4.0.0",
       "pytest-benchmark>=4.0.0",
   ]
   docs = [
       "sphinx>=5.0.0",
       "sphinx-rtd-theme>=1.0.0",
       "myst-parser>=0.18.0",
   ]
   
   [project.urls]
   Homepage = "https://github.com/yourusername/calculator-pro"
   Documentation = "https://calculator-pro.readthedocs.io/"
   Repository = "https://github.com/yourusername/calculator-pro.git"
   "Bug Tracker" = "https://github.com/yourusername/calculator-pro/issues"
   Changelog = "https://github.com/yourusername/calculator-pro/blob/main/CHANGELOG.md"
   
   [project.scripts]
   calculator = "calculator.cli:main"
   calc = "calculator.cli:main"
   
   [project.entry-points."calculator.plugins"]
   basic = "calculator.operations"
   scientific = "calculator.scientific"
   
   [tool.setuptools]
   package-dir = {"" = "src"}
   
   [tool.setuptools.packages.find]
   where = ["src"]
   
   [tool.setuptools.package-data]
   calculator = ["config/*.env", "py.typed"]
   
   [tool.black]
   line-length = 88
   target-version = ['py38', 'py39', 'py310', 'py311']
   include = '\.pyi?$'
   exclude = '''
   /(
       \.git
     | \.hg
     | \.mypy_cache
     | \.tox
     | \.venv
     | _build
     | buck-out
     | build
     | dist
   )/
   '''
   
   [tool.mypy]
   python_version = "3.8"
   warn_return_any = true
   warn_unused_configs = true
   disallow_untyped_defs = true
   disallow_incomplete_defs = true
   check_untyped_defs = true
   disallow_untyped_decorators = true
   no_implicit_optional = true
   warn_redundant_casts = true
   warn_unused_ignores = true
   warn_no_return = true
   warn_unreachable = true
   strict_equality = true
   
   [tool.pytest.ini_options]
   minversion = "7.0"
   addopts = [
       "--strict-markers",
       "--strict-config",
       "--cov=calculator",
       "--cov-report=term-missing",
       "--cov-report=html",
       "--cov-report=xml",
   ]
   testpaths = ["tests"]
   markers = [
       "slow: marks tests as slow (deselect with '-m \"not slow\"')",
       "integration: marks tests as integration tests",
       "unit: marks tests as unit tests",
   ]
   
   [tool.coverage.run]
   source = ["src/calculator"]
   
   [tool.coverage.report]
   exclude_lines = [
       "pragma: no cover",
       "def __repr__",
       "if self.debug:",
       "if settings.DEBUG",
       "raise AssertionError",
       "raise NotImplementedError",
       "if 0:",
       "if __name__ == .__main__.:",
       "class .*\bProtocol\):",
       "@(abc\.)?abstractmethod",
   ]
   
   [tool.pylint.messages_control]
   disable = [
       "missing-docstring",
       "too-few-public-methods",
   ]
   
   [tool.pylint.format]
   max-line-length = 88
   ```

### Task 2.2: Create Package Documentation

1. Create `LICENSE`:
   ```
   MIT License
   
   Copyright (c) 2024 Your Name
   
   Permission is hereby granted, free of charge, to any person obtaining a copy
   of this software and associated documentation files (the "Software"), to deal
   in the Software without restriction, including without limitation the rights
   to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
   copies of the Software, and to permit persons to whom the Software is
   furnished to do so, subject to the following conditions:
   
   The above copyright notice and this permission notice shall be included in all
   copies or substantial portions of the Software.
   
   THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
   IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
   FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
   AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
   LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
   OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
   SOFTWARE.
   ```

2. Create `CHANGELOG.md`:
   ```markdown
   # Changelog
   
   All notable changes to this project will be documented in this file.
   
   The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
   and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).
   
   ## [Unreleased]
   
   ## [1.0.0] - 2024-01-XX
   
   ### Added
   - Initial release of Calculator Pro
   - Basic arithmetic operations (add, subtract, multiply, divide)
   - Scientific operations (sqrt, power, trigonometric functions)
   - Command-line interface with interactive mode
   - Configuration management with environment support
   - Comprehensive logging system
   - Memory operations
   - Calculation history
   - Professional package structure
   
   ### Features
   - Modern Python packaging with pyproject.toml
   - Type hints throughout codebase
   - Comprehensive test suite with >90% coverage
   - CI/CD pipeline with GitHub Actions
   - Security scanning and code quality checks
   - Documentation with examples
   
   ### Technical
   - Python 3.8+ support
   - Pydantic for configuration management
   - Cryptography for secrets management
   - Black for code formatting
   - Pylint and MyPy for code quality
   - Pytest for testing
   ```

3. Update `README.md` for package distribution:
   ```markdown
   # Calculator Pro
   
   A professional calculator library with advanced features for Python.
   
   [![PyPI version](https://badge.fury.io/py/calculator-pro.svg)](https://badge.fury.io/py/calculator-pro)
   [![Python versions](https://img.shields.io/pypi/pyversions/calculator-pro.svg)](https://pypi.org/project/calculator-pro/)
   [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
   [![Tests](https://github.com/yourusername/calculator-pro/workflows/Tests/badge.svg)](https://github.com/yourusername/calculator-pro/actions)
   [![Coverage](https://codecov.io/gh/yourusername/calculator-pro/branch/main/graph/badge.svg)](https://codecov.io/gh/yourusername/calculator-pro)
   
   ## Features
   
   - **Basic Operations**: Addition, subtraction, multiplication, division
   - **Scientific Functions**: Square root, power, trigonometric functions
   - **Memory Operations**: Store, recall, and clear memory
   - **History Management**: Track calculation history with configurable limits
   - **Configuration Management**: Environment-based configuration with validation
   - **Logging**: Comprehensive logging with rotation and levels
   - **CLI Interface**: Command-line tool with interactive mode
   - **Type Safety**: Full type hints and mypy support
   - **Modern Packaging**: Built with pyproject.toml and setuptools
   
   ## Installation
   
   ### From PyPI
   
   ```bash
   pip install calculator-pro
   ```
   
   ### From Source
   
   ```bash
   git clone https://github.com/yourusername/calculator-pro.git
   cd calculator-pro
   pip install -e .
   ```
   
   ### Development Installation
   
   ```bash
   git clone https://github.com/yourusername/calculator-pro.git
   cd calculator-pro
   pip install -e ".[dev]"
   ```
   
   ## Quick Start
   
   ### As a Library
   
   ```python
   from calculator import Calculator
   
   calc = Calculator()
   
   # Basic operations
   result = calc.add(5, 3)
   print(result)  # 8.0
   
   # Scientific operations
   result = calc.square_root(16)
   print(result)  # 4.0
   
   # Memory operations
   calc.memory_store(42)
   value = calc.memory_recall()
   print(value)  # 42.0
   
   # History
   history = calc.get_history()
   for entry in history:
       print(entry)
   ```
   
   ### Command Line Interface
   
   ```bash
   # Basic calculations
   calculator add 5 3
   calculator divide 10 2
   
   # Interactive mode
   calculator --interactive
   
   # Scientific operations
   calculator sqrt 16
   calculator power 2 8
   
   # With custom precision
   calculator --precision 4 divide 22 7
   ```
   
   ### Interactive Mode
   
   ```bash
   $ calculator --interactive
   Calculator v1.0.0 - Interactive Mode
   Type 'help' for commands, 'quit' to exit
   ----------------------------------------
   calc> add 5 3
   Result: 8.0
   calc> sqrt 16
   Result: 4.0
   calc> history
   Calculation History:
     1. 5.0 + 3.0 = 8.0
     2. √16.0 = 4.0
   calc> quit
   Goodbye!
   ```
   
   ## Configuration
   
   Calculator Pro supports environment-based configuration:
   
   ```bash
   # Set precision
   export CALC_PRECISION=10
   
   # Set logging level
   export LOG_LEVEL=DEBUG
   
   # Set history limit
   export CALC_MAX_HISTORY=50
   ```
   
   See the [Configuration Guide](docs/configuration.md) for complete details.
   
   ## Development
   
   ### Setup Development Environment
   
   ```bash
   git clone https://github.com/yourusername/calculator-pro.git
   cd calculator-pro
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   pip install -e ".[dev]"
   ```
   
   ### Running Tests
   
   ```bash
   # Run all tests
   pytest
   
   # Run with coverage
   pytest --cov=calculator
   
   # Run specific test file
   pytest tests/test_calculator.py
   ```
   
   ### Code Quality
   
   ```bash
   # Format code
   black src tests
   
   # Lint code
   pylint src/calculator
   
   # Type checking
   mypy src/calculator
   ```
   
   ### Pre-commit Hooks
   
   ```bash
   pre-commit install
   pre-commit run --all-files
   ```
   
   ## Contributing
   
   1. Fork the repository
   2. Create a feature branch (`git checkout -b feature/amazing-feature`)
   3. Commit your changes (`git commit -m 'Add amazing feature'`)
   4. Push to the branch (`git push origin feature/amazing-feature`)
   5. Open a Pull Request
   
   ## License
   
   This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
   
   ## Changelog
   
   See [CHANGELOG.md](CHANGELOG.md) for a list of changes.
   ```

## Part 3: Building and Testing the Package

### Task 3.1: Local Package Build

1. Create `MANIFEST.in`:
   ```
   include README.md
   include LICENSE
   include CHANGELOG.md
   include pyproject.toml
   
   recursive-include src/calculator *.py
   recursive-include config *.env
   recursive-include tests *.py
   
   global-exclude *.pyc
   global-exclude __pycache__
   global-exclude .git*
   global-exclude .pytest_cache
   global-exclude *.egg-info
   ```

2. Create build script `scripts/build.py`:
   ```python
   #!/usr/bin/env python3
   """Build script for the calculator package."""
   
   import subprocess
   import sys
   from pathlib import Path
   
   
   def run_command(command, description):
       """Run a command and handle errors."""
       print(f"\n{description}...")
       print(f"Running: {command}")
       
       result = subprocess.run(command, shell=True, capture_output=True, text=True)
       
       if result.returncode != 0:
           print(f"Error: {description} failed")
           print(f"Stdout: {result.stdout}")
           print(f"Stderr: {result.stderr}")
           return False
       
       print(f"Success: {description} completed")
       if result.stdout:
           print(f"Output: {result.stdout}")
       return True
   
   
   def main():
       """Main build process."""
       project_root = Path(__file__).parent.parent
       
       print(f"Building calculator package from {project_root}")
       
       # Change to project directory
       import os
       os.chdir(project_root)
       
       # Clean previous builds
       if not run_command("rm -rf build dist *.egg-info", "Cleaning previous builds"):
           return 1
       
       # Install build dependencies
       if not run_command("pip install --upgrade pip build twine", "Installing build tools"):
           return 1
       
       # Run tests
       if not run_command("python -m pytest tests/", "Running tests"):
           print("Warning: Tests failed, but continuing build...")
       
       # Build package
       if not run_command("python -m build", "Building package"):
           return 1
       
       # Check package
       if not run_command("python -m twine check dist/*", "Checking package"):
           return 1
       
       print("\n✅ Package built successfully!")
       print("Built files:")
       for file in Path("dist").glob("*"):
           print(f"  - {file}")
       
       return 0
   
   
   if __name__ == "__main__":
       sys.exit(main())
   ```

3. Make build script executable and run:
   ```bash
   chmod +x scripts/build.py
   python scripts/build.py
   ```

### Task 3.2: Package Installation Testing

1. Create `scripts/test_installation.py`:
   ```python
   #!/usr/bin/env python3
   """Test package installation in different environments."""
   
   import subprocess
   import sys
   import tempfile
   import venv
   from pathlib import Path
   
   
   def create_test_venv(venv_path):
       """Create a test virtual environment."""
       print(f"Creating virtual environment at {venv_path}")
       venv.create(venv_path, with_pip=True)
   
   
   def run_in_venv(venv_path, command):
       """Run a command in the virtual environment."""
       if sys.platform == "win32":
           python_path = venv_path / "Scripts" / "python"
           pip_path = venv_path / "Scripts" / "pip"
       else:
           python_path = venv_path / "bin" / "python"
           pip_path = venv_path / "bin" / "pip"
       
       if command.startswith("pip"):
           command = command.replace("pip", str(pip_path))
       elif command.startswith("python"):
           command = command.replace("python", str(python_path))
       
       result = subprocess.run(command, shell=True, capture_output=True, text=True)
       return result
   
   
   def test_wheel_installation(dist_path):
       """Test installing from wheel."""
       with tempfile.TemporaryDirectory() as temp_dir:
           venv_path = Path(temp_dir) / "test_venv"
           create_test_venv(venv_path)
           
           # Find wheel file
           wheel_files = list(dist_path.glob("*.whl"))
           if not wheel_files:
               print("❌ No wheel file found")
               return False
           
           wheel_file = wheel_files[0]
           print(f"Testing wheel installation: {wheel_file}")
           
           # Install wheel
           result = run_in_venv(venv_path, f"pip install {wheel_file}")
           if result.returncode != 0:
               print(f"❌ Wheel installation failed: {result.stderr}")
               return False
           
           # Test import
           result = run_in_venv(venv_path, "python -c 'import calculator; print(calculator.__version__)'")
           if result.returncode != 0:
               print(f"❌ Import test failed: {result.stderr}")
               return False
           
           print(f"✅ Import successful: {result.stdout.strip()}")
           
           # Test CLI
           result = run_in_venv(venv_path, "calculator add 2 3")
           if result.returncode != 0:
               print(f"❌ CLI test failed: {result.stderr}")
               return False
           
           print(f"✅ CLI test successful: {result.stdout.strip()}")
           
           return True
   
   
   def test_sdist_installation(dist_path):
       """Test installing from source distribution."""
       with tempfile.TemporaryDirectory() as temp_dir:
           venv_path = Path(temp_dir) / "test_venv"
           create_test_venv(venv_path)
           
           # Find source distribution
           sdist_files = list(dist_path.glob("*.tar.gz"))
           if not sdist_files:
               print("❌ No source distribution found")
               return False
           
           sdist_file = sdist_files[0]
           print(f"Testing source distribution installation: {sdist_file}")
           
           # Install source distribution
           result = run_in_venv(venv_path, f"pip install {sdist_file}")
           if result.returncode != 0:
               print(f"❌ Source distribution installation failed: {result.stderr}")
               return False
           
           # Test import
           result = run_in_venv(venv_path, "python -c 'import calculator; print(calculator.__version__)'")
           if result.returncode != 0:
               print(f"❌ Import test failed: {result.stderr}")
               return False
           
           print(f"✅ Import successful: {result.stdout.strip()}")
           
           return True
   
   
   def main():
       """Main test function."""
       project_root = Path(__file__).parent.parent
       dist_path = project_root / "dist"
       
       if not dist_path.exists():
           print("❌ No dist directory found. Run build first.")
           return 1
       
       print("Testing package installations...")
       
       # Test wheel installation
       if not test_wheel_installation(dist_path):
           return 1
       
       # Test source distribution installation
       if not test_sdist_installation(dist_path):
           return 1
       
       print("\n✅ All installation tests passed!")
       return 0
   
   
   if __name__ == "__main__":
       sys.exit(main())
   ```

## Part 4: Publishing to PyPI

### Task 4.1: PyPI Test Environment

1. Create account on [Test PyPI](https://test.pypi.org/) and [PyPI](https://pypi.org/)

2. Create `scripts/publish.py`:
   ```python
   #!/usr/bin/env python3
   """Publishing script for the calculator package."""
   
   import subprocess
   import sys
   import os
   from pathlib import Path
   
   
   def run_command(command, description):
       """Run a command and handle errors."""
       print(f"\n{description}...")
       print(f"Running: {command}")
       
       result = subprocess.run(command, shell=True)
       
       if result.returncode != 0:
           print(f"Error: {description} failed")
           return False
       
       print(f"Success: {description} completed")
       return True
   
   
   def check_credentials(repository):
       """Check if credentials are available."""
       if repository == "testpypi":
           token_var = "TEST_PYPI_TOKEN"
       else:
           token_var = "PYPI_TOKEN"
       
       if not os.getenv(token_var):
           print(f"❌ {token_var} environment variable not set")
           print(f"Set it with: export {token_var}=your_token_here")
           return False
       
       return True
   
   
   def publish_to_repository(repository="testpypi"):
       """Publish package to PyPI repository."""
       project_root = Path(__file__).parent.parent
       dist_path = project_root / "dist"
       
       if not dist_path.exists() or not list(dist_path.glob("*")):
           print("❌ No distribution files found. Run build first.")
           return False
       
       if not check_credentials(repository):
           return False
       
       if repository == "testpypi":
           repo_url = "https://test.pypi.org/legacy/"
           token_var = "TEST_PYPI_TOKEN"
       else:
           repo_url = "https://upload.pypi.org/legacy/"
           token_var = "PYPI_TOKEN"
       
       token = os.getenv(token_var)
       
       # Upload to repository
       command = f"python -m twine upload --repository-url {repo_url} --username __token__ --password {token} dist/*"
       
       if not run_command(command, f"Uploading to {repository}"):
           return False
       
       print(f"\n✅ Package published to {repository} successfully!")
       
       if repository == "testpypi":
           print("Test installation with:")
           print("pip install --index-url https://test.pypi.org/simple/ calculator-pro")
       else:
           print("Install with:")
           print("pip install calculator-pro")
       
       return True
   
   
   def main():
       """Main publishing function."""
       if len(sys.argv) < 2:
           print("Usage: python publish.py [testpypi|pypi]")
           return 1
       
       repository = sys.argv[1]
       
       if repository not in ["testpypi", "pypi"]:
           print("Repository must be 'testpypi' or 'pypi'")
           return 1
       
       if repository == "pypi":
           response = input("⚠️  Publishing to production PyPI. Are you sure? (yes/no): ")
           if response.lower() != "yes":
               print("Cancelled.")
               return 0
       
       if publish_to_repository(repository):
           return 0
       else:
           return 1
   
   
   if __name__ == "__main__":
       sys.exit(main())
   ```

### Task 4.2: Automated Versioning

1. Create `scripts/bump_version.py`:
   ```python
   #!/usr/bin/env python3
   """Version bumping script."""
   
   import re
   import sys
   from pathlib import Path
   from datetime import datetime
   
   
   def get_current_version():
       """Get current version from version.py."""
       version_file = Path("src/calculator/version.py")
       content = version_file.read_text()
       match = re.search(r'__version__ = ["\']([^"\']+)["\']', content)
       if match:
           return match.group(1)
       raise ValueError("Could not find version in version.py")
   
   
   def parse_version(version):
       """Parse version string into components."""
       parts = version.split('.')
       if len(parts) != 3:
           raise ValueError("Version must be in format X.Y.Z")
       return [int(part) for part in parts]
   
   
   def bump_version(current_version, bump_type):
       """Bump version based on type."""
       major, minor, patch = parse_version(current_version)
       
       if bump_type == "major":
           major += 1
           minor = 0
           patch = 0
       elif bump_type == "minor":
           minor += 1
           patch = 0
       elif bump_type == "patch":
           patch += 1
       else:
           raise ValueError("Bump type must be 'major', 'minor', or 'patch'")
       
       return f"{major}.{minor}.{patch}"
   
   
   def update_version_file(new_version):
       """Update version in version.py."""
       version_file = Path("src/calculator/version.py")
       content = version_file.read_text()
       
       # Update version
       content = re.sub(
           r'__version__ = ["\'][^"\']+["\']',
           f'__version__ = "{new_version}"',
           content
       )
       
       version_file.write_text(content)
       print(f"✅ Updated version.py to {new_version}")
   
   
   def update_pyproject_toml(new_version):
       """Update version in pyproject.toml."""
       pyproject_file = Path("pyproject.toml")
       content = pyproject_file.read_text()
       
       # Update version
       content = re.sub(
           r'version = ["\'][^"\']+["\']',
           f'version = "{new_version}"',
           content
       )
       
       pyproject_file.write_text(content)
       print(f"✅ Updated pyproject.toml to {new_version}")
   
   
   def update_changelog(new_version):
       """Update changelog with new version."""
       changelog_file = Path("CHANGELOG.md")
       content = changelog_file.read_text()
       
       today = datetime.now().strftime("%Y-%m-%d")
       
       # Replace [Unreleased] with new version
       content = content.replace(
           "## [Unreleased]",
           f"## [Unreleased]\n\n## [{new_version}] - {today}"
       )
       
       changelog_file.write_text(content)
       print(f"✅ Updated CHANGELOG.md with {new_version}")
   
   
   def main():
       """Main version bumping function."""
       if len(sys.argv) != 2:
           print("Usage: python bump_version.py [major|minor|patch]")
           return 1
       
       bump_type = sys.argv[1]
       
       if bump_type not in ["major", "minor", "patch"]:
           print("Bump type must be 'major', 'minor', or 'patch'")
           return 1
       
       try:
           current_version = get_current_version()
           new_version = bump_version(current_version, bump_type)
           
           print(f"Bumping version from {current_version} to {new_version}")
           
           update_version_file(new_version)
           update_pyproject_toml(new_version)
           update_changelog(new_version)
           
           print(f"\n✅ Version bumped successfully!")
           print(f"Next steps:")
           print(f"1. git add -A")
           print(f"2. git commit -m 'Bump version to {new_version}'")
           print(f"3. git tag v{new_version}")
           print(f"4. git push && git push --tags")
           
           return 0
           
       except Exception as e:
           print(f"❌ Error: {e}")
           return 1
   
   
   if __name__ == "__main__":
       sys.exit(main())
   ```

## Part 5: Continuous Integration for Packaging

### Task 5.1: Update GitHub Actions for Publishing

1. Create `.github/workflows/publish.yml`:
   ```yaml
   name: Publish Package
   
   on:
     release:
       types: [published]
     workflow_dispatch:
       inputs:
         repository:
           description: 'Repository to publish to'
           required: true
           default: 'testpypi'
           type: choice
           options:
           - testpypi
           - pypi
   
   jobs:
     publish:
       runs-on: ubuntu-latest
       environment: 
         name: ${{ github.event.inputs.repository || 'pypi' }}
       
       steps:
       - name: Checkout code
         uses: actions/checkout@v4
       
       - name: Set up Python
         uses: actions/setup-python@v4
         with:
           python-version: '3.10'
           cache: 'pip'
       
       - name: Install dependencies
         run: |
           python -m pip install --upgrade pip
           pip install build twine
       
       - name: Build package
         run: python -m build
       
       - name: Check package
         run: python -m twine check dist/*
       
       - name: Publish to Test PyPI
         if: github.event.inputs.repository == 'testpypi' || github.event_name == 'workflow_dispatch'
         env:
           TWINE_USERNAME: __token__
           TWINE_PASSWORD: ${{ secrets.TEST_PYPI_API_TOKEN }}
           TWINE_REPOSITORY_URL: https://test.pypi.org/legacy/
         run: python -m twine upload dist/*
       
       - name: Publish to PyPI
         if: github.event_name == 'release' || github.event.inputs.repository == 'pypi'
         env:
           TWINE_USERNAME: __token__
           TWINE_PASSWORD: ${{ secrets.PYPI_API_TOKEN }}
         run: python -m twine upload dist/*
   ```

2. Update `.github/workflows/ci.yml` to include package testing:
   ```yaml
   # Add this job to your existing CI workflow
   test-package:
     runs-on: ubuntu-latest
     needs: test
     
     steps:
     - name: Checkout code
       uses: actions/checkout@v4
     
     - name: Set up Python
       uses: actions/setup-python@v4
       with:
         python-version: '3.10'
         cache: 'pip'
     
     - name: Install build tools
       run: |
         python -m pip install --upgrade pip
         pip install build twine
     
     - name: Build package
       run: python -m build
     
     - name: Check package
       run: python -m twine check dist/*
     
     - name: Test wheel installation
       run: |
         python -m venv test_env
         source test_env/bin/activate
         pip install dist/*.whl
         calculator add 2 3
         python -c "import calculator; print(calculator.__version__)"
     
     - name: Upload package artifacts
       uses: actions/upload-artifact@v3
       with:
         name: package-distributions
         path: dist/
   ```

## Common Pitfalls

- **Version conflicts**: Ensure version consistency across all files
- **Missing dependencies**: Test installation in clean environments
- **Entry point errors**: Verify CLI scripts work after installation
- **Metadata issues**: Check that all required metadata is included
- **Security tokens**: Never commit API tokens to version control

## What Success Looks Like

By the end of this assignment, you should have:
- A professionally packaged Python project with modern structure
- Working command-line interface with multiple entry points
- Successful local builds and installations
- Package published to Test PyPI
- Automated CI/CD pipeline for packaging and publishing
- Comprehensive documentation for users and contributors

## Self-Assessment Questions

1. What are the advantages of pyproject.toml over setup.py?
2. How does semantic versioning help users understand changes?
3. What security considerations apply to package distribution?
4. How do entry points work and why are they useful?
5. What makes a package "professional" quality?

## Submission Requirements

Submit the following:

1. Your complete packaged calculator project
2. Evidence of successful package builds and installations
3. Screenshots of your package on Test PyPI
4. CI/CD pipeline running successfully
5. A reflection document (400-600 words) addressing:
   - How proper packaging changes software distribution
   - The importance of semantic versioning in dependency management
   - Security considerations for package publishing
   - How these practices enable code reuse and collaboration

## Additional Resources

- [Python Packaging User Guide](https://packaging.python.org/)
- [PEP 517 - A build-system independent format](https://peps.python.org/pep-0517/)
- [PEP 518 - Specifying Minimum Build System Requirements](https://peps.python.org/pep-0518/)
- [Semantic Versioning](https://semver.org/)
- [PyPI Help Documentation](https://pypi.org/help/)
- [Twine Documentation](https://twine.readthedocs.io/)

````