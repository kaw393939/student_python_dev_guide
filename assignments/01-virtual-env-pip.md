# Assignment 1: Virtual Environments and Package Management

## Learning Objectives
- Understand the purpose of virtual environments
- Create and activate Python virtual environments
- Install and manage packages with pip
- Manage package versions and dependencies
- Create and use requirements.txt files

## Background

When developing Python applications, you'll often need specific versions of libraries. Using virtual environments allows you to create isolated spaces for your projects, avoiding conflicts between different projects' dependencies.

## Prerequisites
- Python 3.6+ installed on your system
- Terminal/Command Prompt access

## Part 1: Creating Your First Virtual Environment

### Task 1.1: Create a Virtual Environment

1. Create a new directory for your project:
   ```bash
   mkdir my_first_project
   cd my_first_project
   ```

2. Create a virtual environment:
   ```bash
   # On macOS/Linux/Windows
   python -m venv venv
   ```

3. Activate the virtual environment:
   ```bash
   # On macOS/Linux
   source venv/bin/activate
   
   # On Windows
   venv\Scripts\activate
   ```

4. Verify activation:
   ```bash
   # The command prompt should now show (venv) at the beginning
   # Check Python location to confirm it's using the virtual environment
   which python  # On Windows: where python
   ```

### Task 1.2: Deactivate and Reactivate

1. Deactivate the virtual environment:
   ```bash
   deactivate
   ```

2. Observe that the `(venv)` prefix is gone.

3. Reactivate the virtual environment:
   ```bash
   # On macOS/Linux
   source venv/bin/activate
   
   # On Windows
   venv\Scripts\activate
   ```

## Part 2: Package Management with pip

### Task 2.1: Installing Packages

1. Ensure your virtual environment is activated.

2. Install the `requests` package:
   ```bash
   pip install requests
   ```

3. List installed packages:
   ```bash
   pip list
   ```

4. Install a specific version:
   ```bash
   pip install requests==2.25.1
   ```

5. Verify the version changed:
   ```bash
   pip list
   ```

### Task 2.2: Using Installed Packages

1. Create a file named `weather.py`:
   ```python
   import requests

   def get_weather(city):
       """Get current weather for a city"""
       api_key = "demo_key"  # For demo purposes only
       url = f"https://api.openweathermap.org/data/2.5/weather?q={city}&appid={api_key}&units=metric"
       
       # Using the requests library we installed
       response = requests.get(url)
       
       if response.status_code == 200:
           data = response.json()
           return f"The temperature in {city} is {data['main']['temp']}°C"
       else:
           return f"Error: Could not retrieve weather for {city}"

   if __name__ == "__main__":
       city = input("Enter a city name: ")
       print(get_weather(city))
   ```

2. Run the script (without an API key, it will show an error but that's expected for this exercise):
   ```bash
   python weather.py
   ```

## Part 3: Managing Dependencies

### Task 3.1: Creating a requirements.txt File

1. Generate a requirements.txt file:
   ```bash
   pip freeze > requirements.txt
   ```

2. Examine the contents of requirements.txt:
   ```bash
   cat requirements.txt  # On Windows: type requirements.txt
   ```

### Task 3.2: Using requirements.txt

1. Deactivate and delete your virtual environment:
   ```bash
   deactivate
   rm -rf venv  # On Windows: rmdir /s /q venv
   ```

2. Create a new virtual environment:
   ```bash
   python -m venv new_venv
   ```

3. Activate the new environment:
   ```bash
   # On macOS/Linux
   source new_venv/bin/activate
   
   # On Windows
   new_venv\Scripts\activate
   ```

4. Install dependencies from requirements.txt:
   ```bash
   pip install -r requirements.txt
   ```

5. Verify that all packages were installed:
   ```bash
   pip list
   ```

## Part 4: Advanced Package Management

### Task 4.1: Understanding Version Specifiers

1. Edit your requirements.txt file to use version specifiers:
   ```
   requests>=2.25.0,<2.26.0
   ```

2. Reinstall with the updated requirements:
   ```bash
   pip install -r requirements.txt
   ```

3. Research and document the meaning of these version specifiers:
   - `==`: Exact version
   - `>=`: Greater than or equal to
   - `<=`: Less than or equal to
   - `~=`: Compatible release
   - `!=`: Not equal to

### Task 4.2: Exploring pip Commands

1. Try each of these commands and document what they do:
   ```bash
   pip show requests
   pip check
   pip install --upgrade requests
   pip uninstall requests
   ```

## Submission Requirements

Create a text file named `assignment1_answers.txt` with the following:

1. Screenshots or terminal output showing:
   - Virtual environment activation
   - Package installation
   - Running your weather.py script
   - Creating and using requirements.txt

2. Written answers to these questions:
   - Why are virtual environments important for Python development?
   - What is the difference between `pip install package` and `pip install -r requirements.txt`?
   - How would you specify that your project needs any version of requests that is at least 2.20.0 but less than 3.0.0?
   - What happens if two projects on your computer need different versions of the same library?

## Additional Resources

- [Python Virtual Environments: A Primer](https://realpython.com/python-virtual-environments-a-primer/)
- [pip User Guide](https://pip.pypa.io/en/stable/user_guide/)
- [Python Packaging User Guide](https://packaging.python.org/en/latest/tutorials/installing-packages/)
