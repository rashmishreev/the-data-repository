# Setup Virtual Environment and Install Dependencies

This guide will walk you through setting up a virtual environment and installing dependencies for your project.

## 1. Generate `requirements.txt`

Use the command `pip freeze` to list all installed packages and their versions, and then redirect the output to a `requirements.txt` file.

`pip freeze > requirements.txt`


## 2. Check the Contents of requirements.txt

You can check the contents of the requirements.txt file to ensure it lists all your project dependencies.

`cat requirements.txt`

## 3. Create a New Virtual Environment

Create a new virtual environment for your project. This isolates your project dependencies from your system Python environment.

`python -m venv .venv-py-datascience`

## 4. Activate the Virtual Environment

Activate the newly created virtual environment. The activation command differs based on your operating system.

On macOS/Linux:

`source .venv-py-datascience/bin/activate`

On Windows:

`.venv-py-datascience\Scripts\activate`

## 5. Install Dependencies

Once the virtual environment is activated, you can install the dependencies listed in requirements.txt.

`pip install -r requirements.txt`

By following these steps, you'll be able to set up a virtual environment and manage dependencies effectively for your project.
