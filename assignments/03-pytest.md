# Assignment 3: Testing with Pytest

## Learning Objectives
- Understand the importance of automated testing
- Learn how to write and run tests using pytest
- Create test fixtures and parameterized tests
- Measure test coverage
- Apply test-driven development (TDD) principles

## Background

Testing is a critical part of software development. It helps ensure your code works as expected and continues to work as you make changes. Pytest is a popular testing framework for Python that makes it easy to write simple and scalable tests.

## Prerequisites
- Python 3.6+ installed
- Virtual environment knowledge (from Assignment 1)
- Understanding of code quality (from Assignment 2)
- Basic Python programming skills

## Part 1: Setting Up Pytest

### Task 1.1: Prepare Your Environment

1. Create a new directory for this assignment:
   ```bash
   mkdir pytest_assignment
   cd pytest_assignment
   ```

2. Create and activate a virtual environment:
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. Install pytest and pytest-cov:
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
   echo "def test_passing(): assert True" > test_simple.py
   pytest
   ```

## Part 2: Writing Basic Tests

### Task 2.1: Testing a Simple Function

1. Create a file named `math_operations.py` with the following content:

   ```python
   """Basic mathematical operations."""
   
   def add(a, b):
       """Add two numbers and return the result."""
       return a + b
   
   def subtract(a, b):
       """Subtract b from a and return the result."""
       return a - b
   
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

2. Create a test file named `test_math_operations.py`:

   ```python
   """Tests for math operations."""
   from math_operations import add, subtract, multiply, divide
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
   
   # Add tests for multiply and divide here
   ```

3. Complete the test file by adding tests for `multiply` and `divide` functions.

4. Run the tests:
   ```bash
   pytest
   ```

### Task 2.2: Testing Exceptions

1. Add a test function to test the exception raised by the divide function:

   ```python
   def test_divide_by_zero():
       """Test that dividing by zero raises an exception."""
       with pytest.raises(ZeroDivisionError):
           divide(1, 0)
   ```

2. Run the tests again:
   ```bash
   pytest
   ```

## Part 3: Advanced Testing Features

### Task 3.1: Test Fixtures

1. Create a file named `user.py`:

   ```python
   """User management module."""
   
   class User:
       """Simple User class."""
       
       def __init__(self, user_id, username, email):
           """Initialize a user with id, username, and email."""
           self.user_id = user_id
           self.username = username
           self.email = email
           self.is_active = True
           self.posts = []
       
       def deactivate(self):
           """Deactivate the user."""
           self.is_active = False
       
       def add_post(self, post):
           """Add a post to the user's posts."""
           self.posts.append(post)
           return len(self.posts)
       
       def get_posts(self):
           """Return the user's posts."""
           return self.posts
   ```

2. Create a test file named `test_user.py`:

   ```python
   """Tests for the User class."""
   import pytest
   from user import User
   
   @pytest.fixture
   def active_user():
       """Create and return an active user for testing."""
       return User(1, "testuser", "test@example.com")
   
   @pytest.fixture
   def inactive_user():
       """Create and return an inactive user for testing."""
       user = User(2, "inactive", "inactive@example.com")
       user.deactivate()
       return user
   
   def test_user_initialization(active_user):
       """Test that a user is initialized with the correct attributes."""
       assert active_user.user_id == 1
       assert active_user.username == "testuser"
       assert active_user.email == "test@example.com"
       assert active_user.is_active is True
       assert active_user.posts == []
   
   def test_user_deactivation(active_user):
       """Test that a user can be deactivated."""
       active_user.deactivate()
       assert active_user.is_active is False
   
   # Add more tests for add_post and get_posts methods using the fixtures
   ```

3. Complete the test file by adding tests for the `add_post` and `get_posts` methods.

4. Run the tests:
   ```bash
   pytest
   ```

### Task 3.2: Parameterized Tests

1. Add a parameterized test to `test_math_operations.py`:

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
   ```

2. Create similar parameterized tests for the other math functions.

3. Run the tests:
   ```bash
   pytest
   ```

## Part 4: Test Coverage

### Task 4.1: Measuring Coverage

1. Run your tests with coverage:
   ```bash
   pytest --cov=.
   ```

2. Generate an HTML coverage report:
   ```bash
   pytest --cov=. --cov-report=html
   ```

3. Examine the coverage report. Are there any parts of your code not being tested?

### Task 4.2: Improving Coverage

1. Add additional tests to improve your coverage.

2. Run coverage again to verify improvement.

## Part 5: Test-Driven Development

### Task 5.1: Writing Tests First

1. Create a new file named `test_string_utils.py`:

   ```python
   """Tests for string utility functions that don't exist yet."""
   import pytest
   from string_utils import reverse_string, count_vowels, is_palindrome
   
   def test_reverse_string():
       """Test that strings are correctly reversed."""
       assert reverse_string("hello") == "olleh"
       assert reverse_string("python") == "nohtyp"
       assert reverse_string("") == ""
       assert reverse_string("a") == "a"
   
   def test_count_vowels():
       """Test that vowels are correctly counted."""
       assert count_vowels("hello") == 2
       assert count_vowels("python") == 1
       assert count_vowels("aeiou") == 5
       assert count_vowels("") == 0
       assert count_vowels("bcdfg") == 0
   
   def test_is_palindrome():
       """Test that palindromes are correctly identified."""
       assert is_palindrome("radar") is True
       assert is_palindrome("python") is False
       assert is_palindrome("") is True
       assert is_palindrome("a") is True
       assert is_palindrome("Radar") is False  # Case-sensitive
   ```

2. Run the tests. They will fail because the functions don't exist yet:
   ```bash
   pytest test_string_utils.py -v
   ```

### Task 5.2: Implementing the Code

1. Create a file named `string_utils.py` and implement the functions to make the tests pass:

   ```python
   """Utility functions for string manipulation."""
   
   def reverse_string(s):
       """
       Reverse a string.
       
       Args:
           s: The string to reverse
           
       Returns:
           The reversed string
       """
       # Implement this function
       pass
   
   def count_vowels(s):
       """
       Count the number of vowels in a string.
       
       Args:
           s: The string to count vowels in
           
       Returns:
           The number of vowels in the string
       """
       # Implement this function
       pass
   
   def is_palindrome(s):
       """
       Check if a string is a palindrome.
       
       A palindrome is a string that reads the same forwards and backwards.
       
       Args:
           s: The string to check
           
       Returns:
           True if the string is a palindrome, False otherwise
       """
       # Implement this function
       pass
   ```

2. Implement each function so that the tests pass.

3. Run the tests again to verify they pass:
   ```bash
   pytest test_string_utils.py -v
   ```

## Part 6: Integration with Continuous Integration

### Task 6.1: Setting Up a Test Configuration

1. Create a file named `pytest.ini`:

   ```ini
   [pytest]
   testpaths = .
   python_files = test_*.py
   python_functions = test_*
   python_classes = Test*
   ```

2. Create a `.github/workflows/test.yml` file (for GitHub Actions):

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
       - uses: actions/checkout@v2
       - name: Set up Python
         uses: actions/setup-python@v2
         with:
           python-version: '3.10'
       - name: Install dependencies
         run: |
           python -m pip install --upgrade pip
           pip install pytest pytest-cov
           if [ -f requirements.txt ]; then pip install -r requirements.txt; fi
       - name: Test with pytest
         run: |
           pytest --cov=. --cov-report=xml
       - name: Upload coverage to Codecov
         uses: codecov/codecov-action@v2
   ```

## Submission Requirements

Submit the following files:

1. All Python modules (`.py` files) you created
2. All test files (beginning with `test_`)
3. Your pytest configuration and CI workflow files
4. A screenshot of your test results with coverage
5. A text file named `tdd_reflection.txt` answering these questions:
   - What was challenging about writing tests before implementing the code?
   - How did the test-first approach influence your implementation?
   - In what ways does automated testing improve code quality?
   - How would you integrate testing into your development workflow?

## Additional Resources

- [Pytest Documentation](https://docs.pytest.org/)
- [Python Testing with pytest (Book by Brian Okken)](https://pragprog.com/titles/bopytest/python-testing-with-pytest/)
- [Test-Driven Development with Python](https://www.obeythetestinggoat.com/)
- [Codecov for Python](https://about.codecov.io/language/python/)
