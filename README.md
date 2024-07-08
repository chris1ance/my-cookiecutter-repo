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
    conda create -n "$env_name" python=3.11 cookiecutter
fi

# Activate the environment
conda activate "$env_name"

# Create repo 
cookiecutter https://github.com/chris1ance/my-cookiecutter-repo.git --no-input project_slug="$project_name"

# Change dir to the new project dir
cd "${project_name}"

# Install basic reqs
pip install -r requirements.txt
pip install -r requirements_dev.txt
pip install -e .

# Git init
git init .

# Set up pre-commit hooks
pre-commit install
```