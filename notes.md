# Use the command `pip freeze`to list all the installed packages and their versions, and then redirect the output to requirements.txt
# Generate the requirements.txt file
pip freeze > requirements.txt

# Check the contents of requirements.txt
cat requirements.txt

# Create a new virtual environment
python -m venv .venv-py-datascience

# Activate virtual environment
> source .venv-py-datascience/bin/activate

# Install Dependencies
pip install -r requirements.txt
