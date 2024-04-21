# Usage Example Shell Script

```bash
#!/bin/bash

# make_repo.sh
# Run this script using command: source make_repo.sh project_name
# Replace project_name with your desired repo name

# Specify the desired environment name
project_name=$1
env_name="${project_name}_env"

# Check if the environment already exists
if conda env list | grep -q "$env_name"; then
    echo "Environment $env_name already exists."
else
    # Create the environment
    conda create -n "$env_name" python=3.11 cookiecutter poetry
fi

# Activate the environment
conda activate "$env_name"

# Create repo 
#cookiecutter my-cookiecutter-repo --no-input project_slug="$project_name"
cookiecutter https://github.com/chris1ance/my-cookiecutter-repo.git --no-input project_slug="$project_name"

# Change dir to the new project dir
cd "${project_name}"

# Set up poetry
poetry init --no-interaction
poetry add `cat requirements.txt`
poetry add `cat requirements_dev.txt` --group dev
poetry update
poetry install

# Git init
git init .

# Set up pre-commit hooks
pre-commit install
```