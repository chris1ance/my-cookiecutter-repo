# Usage Example Shell Script

```bash
#!/bin/bash

# make_repo.sh
# Run this script using command: source make_repo.sh project_name
# Replace project_name with your desired repo name

# Assign parameters
project_name=$1
python_version=$2

# Specify the desired environment name
env_name="${project_name}_env"

# Check if the environment already exists
if conda env list | grep -q "$env_name"; then
    echo "Environment $env_name already exists."
else
    # Create the environment
    conda create -n "$env_name" python="$python_version" cookiecutter
fi

# Activate the environment
conda activate "$env_name"

# Create repo 
cookiecutter https://github.com/chris1ance/my-cookiecutter-repo.git --no-input project_slug="$project_name"

# Change dir to the new project dir
cd "${project_name}"
touch .env

# Install package locally in editable mode, include dev deps
pip install -e .[dev]

# Git init
git init .

# Set up pre-commit hooks
pre-commit install
```