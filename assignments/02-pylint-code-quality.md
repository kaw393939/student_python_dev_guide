# Assignment 2: Code Quality with Pylint

## Learning Objectives
- Understand the importance of code quality and style guidelines
- Learn about PEP 8, Python's style guide
- Install and configure pylint
- Identify and fix common code quality issues
- Integrate pylint into your development workflow

## Background

Writing clean, readable code is essential for collaboration and maintenance. Python has a style guide called PEP 8 that provides conventions for Python code. Pylint is a static code analysis tool that checks if your code follows these conventions and helps identify potential errors.

## Prerequisites
- Python 3.6+ installed
- Virtual environment knowledge (from Assignment 1)
- Basic Python programming skills

## Part 1: Setting Up Pylint

### Task 1.1: Prepare Your Environment

1. Create a new directory for this assignment:
   ```bash
   mkdir pylint_assignment
   cd pylint_assignment
   ```

2. Create and activate a virtual environment:
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. Install pylint:
   ```bash
   pip install pylint
   ```

### Task 1.2: Verify Installation

1. Check pylint version:
   ```bash
   pylint --version
   ```

2. Generate a sample pylint configuration file:
   ```bash
   pylint --generate-rcfile > .pylintrc
   ```

3. Take a moment to examine the generated file:
   ```bash
   # On Linux/macOS
   less .pylintrc
   
   # On Windows
   type .pylintrc | more
   ```

## Part 2: Analyzing Code Quality

### Task 2.1: Analyze Poorly Written Code

1. Create a file named `bad_code.py` with the following content:

   ```python
   # This is intentionally bad code for pylint to analyze
   import sys, os
   
   def DoSomething(x,y):
       if x == y:
         print('x and y are equal')
       if x!=y:
           print("x and y are not equal")
       
       z = x+y
       return z
   
   
   def anotherfunc (a,   b,    c):
       sum = a + b + c
       return sum
   
   
   a = DoSomething(3,4)
   b = DoSomething(3,3)
   unused_var = "This variable is never used"
   
   print(a, b)
   ```

2. Run pylint on this file:
   ```bash
   pylint bad_code.py
   ```

3. Take note of the score and the issues pylint found.

### Task 2.2: Understanding Pylint Messages

1. Create a text file named `pylint_issues.txt`.

2. For each unique type of issue found in `bad_code.py`, add an entry to your text file with:
   - The pylint message code (e.g., C0103)
   - The message text (e.g., "Variable name doesn't conform to snake_case naming style")
   - A brief explanation of why this is an issue
   - How you would fix it

## Part 3: Fixing Code Quality Issues

### Task 3.1: Improve the Code

1. Create a new file named `good_code.py`.

2. Rewrite the code from `bad_code.py` fixing all the issues pylint identified.

3. Run pylint on your improved code:
   ```bash
   pylint good_code.py
   ```

4. Keep refining until you achieve a score of at least 9.0/10.

### Task 3.2: Understanding PEP 8

1. Research PEP 8 guidelines for:
   - Naming conventions
   - Indentation
   - Imports
   - Comments
   - Maximum line length
   - Spacing

2. Add a comment at the top of your `good_code.py` file that summarizes the key PEP 8 guidelines you applied.

## Part 4: Real-world Application

### Task 4.1: Analyzing Existing Code

1. Create a file named `calculator.py` with the following content:

   ```python
   """
   A simple calculator module with basic arithmetic operations.
   """
   class Calculator:
       def add(self, a, b):
           """Add two numbers and return the result."""
           return a + b
       
       def subtract(self, a, b):
           """Subtract b from a and return the result."""
           return a - b
       
       def multiply(self, a, b):
           """Multiply two numbers and return the result."""
           return a * b
       
       def divide(self, a, b):
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
   
   # Example usage
   if __name__ == "__main__":
       calc = Calculator()
       print(calc.add(5, 3))       # 8
       print(calc.subtract(5, 3))  # 2
       print(calc.multiply(5, 3))  # 15
       print(calc.divide(6, 3))    # 2.0
   ```

2. Run pylint on this file:
   ```bash
   pylint calculator.py
   ```

3. Note the score. Is it better than `bad_code.py`? Why?

### Task 4.2: Enhancing the Calculator

1. Modify `calculator.py` to add at least two new methods:
   - `power(a, b)` - Returns a raised to the power of b
   - `remainder(a, b)` - Returns the remainder when a is divided by b

2. Ensure your additions maintain good code quality.

3. Run pylint again to verify your score remains high.

## Part 5: Customizing Pylint

### Task 5.1: Configure Pylint

1. Edit the `.pylintrc` file to:
   - Change the maximum line length to 100
   - Disable the "missing-module-docstring" warning
   - Enable the "bad-whitespace" warning

2. Run pylint on your files again with the updated configuration:
   ```bash
   pylint --rcfile=.pylintrc calculator.py
   ```

3. Note any differences in the output.

### Task 5.2: Inline Control

1. Create a file named `inline_control.py` with the following content:

   ```python
   # This file demonstrates inline pylint controls
   
   # pylint: disable=invalid-name
   x = 10  # This would normally trigger a naming warning
   
   def some_function():
       """Demonstrate pylint inline control."""
       # pylint: disable=unused-variable
       unused = "This won't trigger a warning"
       
       # pylint: enable=unused-variable
       unused2 = "This would trigger a warning"
       
       return "Done"
   
   print(some_function())
   ```

2. Run pylint on this file:
   ```bash
   pylint inline_control.py
   ```

3. Note which warnings appear and which don't.

## Submission Requirements

Submit the following files:

1. `bad_code.py` - The original problematic code
2. `good_code.py` - Your improved version
3. `pylint_issues.txt` - Your analysis of pylint issues
4. `calculator.py` - The enhanced calculator with new methods
5. `.pylintrc` - Your customized pylint configuration
6. A screenshot showing the pylint scores for each of your Python files

Also, answer these questions in a file named `reflection.txt`:

1. How does following PEP 8 and using pylint improve code quality?
2. What was the most surprising or interesting pylint warning you encountered?
3. In what ways might pylint's recommendations be limiting or problematic?
4. How would you integrate pylint into a team development workflow?

## Additional Resources

- [PEP 8 Style Guide](https://pep8.org/)
- [Pylint User Manual](http://pylint.pycqa.org/en/latest/)
- [Google Python Style Guide](https://google.github.io/styleguide/pyguide.html)
- [Real Python: Linting Python Code](https://realpython.com/python-code-quality/)
