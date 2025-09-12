# Assignment 3: Testing with Pytest

## Learning Objectives
- Understand the importance of automated testing
- Learn how to write and run tests using pytest
- Create test fixtures and parameterized tests
- Measure test coverage
- Apply test-driven development (TDD) principles

## Background

Testing is a critical part of software development. It helps ensure your code works as expected and continues to work as you make changes. Pytest is a popular testing framework for Python that makes it easy to write simple and scalable tests.

### Historical Context

Software testing has evolved significantly over the decades:
- **1970s-1980s**: Testing was primarily manual and performed after development
- **1990s**: Unit testing frameworks emerged, but were still cumbersome
- **Early 2000s**: Test-Driven Development (TDD) gained popularity with frameworks like JUnit
- **2004**: The Python `unittest` module was standardized, based on JUnit
- **2010**: Pytest was created to simplify Python testing with less boilerplate code
- **Present**: Testing is considered essential, with many organizations practicing TDD and CI/CD

Pytest revolutionized Python testing by introducing fixtures, parameterization, and a simpler syntax compared to the standard `unittest` module.

## Prerequisites
- Python 3.6+ installed
- Virtual environment knowledge (from Assignment 1)
- Understanding of code quality (from Assignment 2)
- Calculator project from previous assignments

## Part 1: Setting Up Pytest for Our Calculator Project

### Task 1.1: Prepare Your Environment

1. Navigate to your calculator project directory:
   ```bash
   # All platforms
   cd python_calculator
   ```

2. Ensure your virtual environment is activated:
   ```bash
   # On Windows with WSL2, macOS, or Linux:
   source venv/bin/activate
   
   # On Windows Command Prompt:
   venv\Scripts\activate
   
   # On Windows PowerShell:
   .\venv\Scripts\Activate.ps1
   ```

3. Install pytest and pytest-cov if not already installed:
   ```bash
   pip install pytest pytest-cov
   ```

### Task 1.2: Verify Installation

1. Check pytest version:
   ```bash
   pytest --version
   ```

2. Create a simple test file to verify pytest works:
   ```bash
   echo "def test_passing(): assert True" > tests/test_simple.py
   pytest
   ```

## Part 2: Writing Tests for Our Calculator Operations

### Task 2.1: Testing Existing Calculator Operations

1. You should already have the basic calculator operations in `src/calculator/operations.py` from previous assignments. Make sure they include:

   ```python
   def add(a, b):
       """Add two numbers and return the result."""
       return a + b
   
   def subtract(a, b):
       """Subtract b from a and return the result."""
       return a - b
   ```

2. Create a test file `tests/test_operations.py`:

   ```python
   """Tests for calculator operations."""
   from calculator.operations import add, subtract
   import pytest
   
   def test_add():
       """Test the add function."""
       assert add(1, 2) == 3
       assert add(-1, 1) == 0
       assert add(-1, -1) == -2
   
   def test_subtract():
       """Test the subtract function."""
       assert subtract(3, 2) == 1
       assert subtract(2, 3) == -1
       assert subtract(0, 0) == 0
   ```

3. Run the tests:
   ```bash
   # All platforms
   pytest -v tests/test_operations.py
   ```

### Task 2.2: Test-Driven Development for Multiply and Divide

1. Add new tests for multiplication and division that will initially fail:

   ```python
   def test_multiply():
       """Test the multiply function."""
       assert multiply(2, 3) == 6
       assert multiply(-2, 3) == -6
       assert multiply(0, 5) == 0
   
   def test_divide():
       """Test the divide function."""
       assert divide(6, 3) == 2
       assert divide(5, 2) == 2.5
       assert divide(-6, 2) == -3
   
   def test_divide_by_zero():
       """Test that dividing by zero raises an exception."""
       with pytest.raises(ZeroDivisionError):
           divide(1, 0)
   ```

2. Run the tests to see them fail:
   ```bash
   pytest -v tests/test_operations.py
   ```

3. Now implement the missing functions in `src/calculator/operations.py`:

   ```python
   def multiply(a, b):
       """Multiply two numbers and return the result."""
       return a * b
   
   def divide(a, b):
       """
       Divide a by b and return the result.
       
       Args:
           a: The dividend
           b: The divisor
               
       Returns:
           The quotient a/b
               
       Raises:
           ZeroDivisionError: If b is 0
       """
       if b == 0:
           raise ZeroDivisionError("Cannot divide by zero")
       return a / b
   ```

4. Run the tests again to see them pass:
   ```bash
   pytest -v tests/test_operations.py
   ```

## Part 3: Creating a Calculator Class with Test Fixtures

### Task 3.1: Implementing a Calculator Class

1. Create a file `src/calculator/calculator.py`:

   ```python
   """Calculator implementation that uses our operations."""
   from .operations import add, subtract, multiply, divide
   
   class Calculator:
       """Calculator class to perform arithmetic operations."""
       
       def __init__(self):
           """Initialize calculator with memory set to 0."""
           self.memory = 0
       
       def add(self, a, b):
           """Add two numbers."""
           return add(a, b)
       
       def subtract(self, a, b):
           """Subtract b from a."""
           return subtract(a, b)
       
       def multiply(self, a, b):
           """Multiply two numbers."""
           return multiply(a, b)
       
       def divide(self, a, b):
           """Divide a by b."""
           return divide(a, b)
       
       def memory_store(self, value):
           """Store a value in memory."""
           self.memory = value
       
       def memory_recall(self):
           """Recall the value from memory."""
           return self.memory
       
       def memory_clear(self):
           """Clear the memory."""
           self.memory = 0
   ```

### Task 3.2: Using Test Fixtures

1. Create a test file `tests/test_calculator.py`:

   ```python
   """Tests for the Calculator class."""
   import pytest
   from calculator.calculator import Calculator
   
   @pytest.fixture
   def calculator():
       """Create and return a Calculator instance for testing."""
       return Calculator()
   
   def test_calculator_initialization(calculator):
       """Test that calculator initializes with memory set to 0."""
       assert calculator.memory_recall() == 0
   
   def test_calculator_operations(calculator):
       """Test the basic calculator operations."""
       assert calculator.add(1, 2) == 3
       assert calculator.subtract(5, 3) == 2
       assert calculator.multiply(2, 4) == 8
       assert calculator.divide(10, 2) == 5
   
   def test_memory_store_and_recall(calculator):
       """Test the memory store and recall functions."""
       calculator.memory_store(5)
       assert calculator.memory_recall() == 5
   
   def test_memory_clear(calculator):
       """Test the memory clear function."""
       calculator.memory_store(5)
       calculator.memory_clear()
       assert calculator.memory_recall() == 0
   ```

2. Run the tests:
   ```bash
   pytest -v tests/test_calculator.py
   ```

## Part 4: Parameterized Tests and Coverage

### Task 4.1: Adding Parameterized Tests

1. Add parameterized tests to `tests/test_operations.py`:

   ```python
   @pytest.mark.parametrize("a, b, expected", [
       (1, 2, 3),
       (0, 0, 0),
       (-1, 1, 0),
       (100, -100, 0)
   ])
   def test_add_parameterized(a, b, expected):
       """Test add function with multiple inputs."""
       assert add(a, b) == expected
   
   @pytest.mark.parametrize("a, b, expected", [
       (5, 2, 3),
       (0, 0, 0),
       (1, 1, 0),
       (-1, -1, 0)
   ])
   def test_subtract_parameterized(a, b, expected):
       """Test subtract function with multiple inputs."""
       assert subtract(a, b) == expected
   ```

2. Create similar parameterized tests for multiply and divide.

### Task 4.2: Measuring Test Coverage

1. Run your tests with coverage:
   ```bash
   pytest --cov=src/calculator
   ```

2. Generate an HTML coverage report:
   ```bash
   pytest --cov=src/calculator --cov-report=html
   ```

3. Examine the report to identify any untested code and add tests to improve coverage.

## Part 5: Test-Driven Development for Scientific Calculator Features

### Task 5.1: Writing Tests First for New Features

1. Create a new test file `tests/test_scientific.py`:

   ```python
   """Tests for scientific calculator operations."""
   import pytest
   import math
   from calculator.scientific import square_root, power
   
   def test_square_root():
       """Test the square root function."""
       assert square_root(4) == 2
       assert square_root(9) == 3
       assert square_root(2) == pytest.approx(1.414, 0.001)
   
   def test_square_root_negative():
       """Test that square root of negative number raises ValueError."""
       with pytest.raises(ValueError):
           square_root(-1)
   
   def test_power():
       """Test the power function."""
       assert power(2, 3) == 8
       assert power(5, 2) == 25
       assert power(4, 0.5) == 2
   ```

2. Run the tests and watch them fail:
   ```bash
   pytest tests/test_scientific.py -v
   ```

### Task 5.2: Implementing the Scientific Functions

1. Create a new file `src/calculator/scientific.py`:

   ```python
   """Scientific calculator operations."""
   import math
   
   def square_root(x):
       """
       Calculate the square root of x.
       
       Args:
           x: The number to calculate the square root of
           
       Returns:
           The square root of x
           
       Raises:
           ValueError: If x is negative
       """
       if x < 0:
           raise ValueError("Cannot calculate square root of negative number")
       return math.sqrt(x)
   
   def power(base, exponent):
       """
       Calculate base raised to the power of exponent.
       
       Args:
           base: The base number
           exponent: The exponent
           
       Returns:
           base^exponent
       """
       return math.pow(base, exponent)
   ```

2. Run the tests again to see them pass:
   ```bash
   pytest tests/test_scientific.py -v
   ```

3. Update the Calculator class to include these new functions:

   ```python
   # Add to src/calculator/calculator.py
   from .scientific import square_root, power
   
   # Add these methods to the Calculator class
   def square_root(self, x):
       """Calculate the square root of x."""
       return square_root(x)
   
   def power(self, base, exponent):
       """Calculate base raised to the power of exponent."""
       return power(base, exponent)
   ```

4. Add tests for these new calculator methods in `tests/test_calculator.py`.

## Part 6: VS Code Integration and CI/CD

### Task 6.1: Setting Up VS Code for Testing

1. Create a `.vscode/settings.json` file:

   ```json
   {
       "python.testing.pytestEnabled": true,
       "python.testing.unittestEnabled": false,
       "python.testing.nosetestsEnabled": false,
       "python.testing.pytestArgs": [
           "tests"
       ]
   }
   ```

2. Use the Test Explorer in VS Code to run your tests.

### Task 6.2: Setting Up GitHub Actions for Continuous Integration

1. Create a `.github/workflows/test.yml` file:

   ```yaml
   name: Python Tests

   on:
     push:
       branches: [ main ]
     pull_request:
       branches: [ main ]

   jobs:
     test:
       runs-on: ubuntu-latest
       steps:
       - uses: actions/checkout@v3
       - name: Set up Python
         uses: actions/setup-python@v4
         with:
           python-version: '3.10'
       - name: Install dependencies
         run: |
           python -m pip install --upgrade pip
           pip install pytest pytest-cov
           pip install -e .
       - name: Test with pytest
         run: |
           pytest --cov=src --cov-report=xml
       - name: Upload coverage to Codecov
         uses: codecov/codecov-action@v3
   ```

## Submission Requirements

Submit the following:

1. Your complete calculator project including:
   - All Python files in `src/calculator/`
   - All test files in `tests/`
   - At least 90% test coverage

2. A text file named `testing_reflection.txt` answering these questions:
   - How does test-driven development change your approach to writing code?
   - Why is automated testing important in professional software development?
   - How do test fixtures and parameterized tests improve test efficiency?
   - How would you integrate testing into your future projects?

## Additional Resources

- [Pytest Documentation](https://docs.pytest.org/)
- [Python Testing with pytest (Book by Brian Okken)](https://pragprog.com/titles/bopytest/python-testing-with-pytest/)
- [Test-Driven Development with Python](https://www.obeythetestinggoat.com/)
   ```python
   """Calculator with memory and scientific functionality."""
   from .operations import add, subtract, multiply, divide
   from .scientific import square_root, power, sin, cos, tan

   class Calculator:
       """Calculator implementation with memory and scientific functionality."""
       
       def __init__(self):
           """Initialize calculator with zero memory."""
           self.memory = 0
       
       def add(self, a, b):
           """Add two numbers."""
           return add(a, b)
       
       def subtract(self, a, b):
           """Subtract b from a."""
           return subtract(a, b)
       
       def multiply(self, a, b):
           """Multiply two numbers."""
           return multiply(a, b)
       
       def divide(self, a, b):
           """Divide a by b."""
           return divide(a, b)
       
       def memory_store(self, value):
           """Store a value in memory."""
           self.memory = value
       
       def memory_recall(self):
           """Recall the stored value."""
           return self.memory
       
       def memory_clear(self):
           """Clear the memory."""
           self.memory = 0
       
       def square_root(self, x):
           """Calculate the square root of x."""
           return square_root(x)
       
       def power(self, base, exponent):
           """Calculate base raised to the power of exponent."""
           return power(base, exponent)
       
       def sin(self, x):
           """Calculate the sine of x (in radians)."""
           return sin(x)
       
       def cos(self, x):
           """Calculate the cosine of x (in radians)."""
           return cos(x)
       
       def tan(self, x):
           """Calculate the tangent of x (in radians)."""
           return tan(x)
   ```

4. Add tests for these new methods in `tests/test_calculator.py` and run all tests:
   ```bash
   pytest
   ```

## Part 6: VS Code Integration and CI/CD

### Task 6.1: Setting Up VS Code for Testing

1. In VS Code, install the Python and Test Explorer extensions if not already installed.

2. Create a `.vscode/settings.json` file with pytest configuration:

   ```json
   {
       "python.testing.pytestEnabled": true,
       "python.testing.unittestEnabled": false,
       "python.testing.nosetestsEnabled": false,
       "python.testing.pytestArgs": [
           "tests"
       ],
       "python.linting.enabled": true,
       "python.linting.pylintEnabled": true
   }
   ```

3. Use the Test Explorer sidebar in VS Code to run and debug tests.

### Task 6.2: Setting Up GitHub Actions for Continuous Testing

1. Create a `.github/workflows/test.yml` file:

   ```yaml
   name: Python Tests

   on:
     push:
       branches: [ main ]
     pull_request:
       branches: [ main ]

   jobs:
     test:
       runs-on: ubuntu-latest
       strategy:
         matrix:
           python-version: ['3.8', '3.10']
       
       steps:
       - uses: actions/checkout@v3
       - name: Set up Python ${{ matrix.python-version }}
         uses: actions/setup-python@v4
         with:
           python-version: ${{ matrix.python-version }}
       - name: Install dependencies
         run: |
           python -m pip install --upgrade pip
           pip install pytest pytest-cov pylint
           pip install -e .
       - name: Lint with pylint
         run: |
           pylint src tests
       - name: Test with pytest
         run: |
           pytest --cov=src --cov-report=xml
       - name: Upload coverage to Codecov
         uses: codecov/codecov-action@v3
   ```

## Troubleshooting Common Testing Issues

### Issue: Tests Can't Import Your Modules

**Solution:**
- Make sure your project is installed in development mode:
  ```bash
  pip install -e .
  ```
- Ensure you have `__init__.py` files in all directories
- Check your import statements match your directory structure

### Issue: Floating Point Comparison Failures

**Solution:**
- Use `pytest.approx()` for floating point comparisons:
  ```python
  assert 0.1 + 0.2 == pytest.approx(0.3)
  ```

### Issue: Test Discovery Problems

**Solution:**
- Ensure test files and functions follow naming conventions:
  - Files should start with `test_`
  - Functions should start with `test_`
- Check your `pytest.ini` or configuration if you're using custom patterns

### Issue: Coverage Report Shows Missing Lines

**Solution:**
- Add tests for edge cases and error conditions
- Ensure all branches (if/else) are tested
- Check that all functions and methods are called in tests

## Submission Requirements

Submit the following:

1. Your complete calculator project with:
   - All Python modules in `src/calculator/`
   - All test files in `tests/`
   - Working scientific calculator functions
   - At least 90% test coverage

2. A screenshot of your VS Code Test Explorer showing all tests passing

3. A screenshot of your coverage report

4. A text file named `tdd_reflection.txt` answering these questions:
   - What was challenging about writing tests before implementing the code?
   - How did the test-first approach influence your implementation?
   - How can TDD improve the quality of a software project?
   - How would you integrate testing into a team development workflow?
   - How has automated testing evolved over time, and why is it important in modern software development?

## Additional Resources

- [Pytest Documentation](https://docs.pytest.org/)
- [Python Testing with pytest (Book by Brian Okken)](https://pragprog.com/titles/bopytest/python-testing-with-pytest/)
- [Test-Driven Development with Python](https://www.obeythetestinggoat.com/)
- [Codecov for Python](https://about.codecov.io/language/python/)
- [Martin Fowler on Test-Driven Development](https://martinfowler.com/bliki/TestDrivenDevelopment.html)
