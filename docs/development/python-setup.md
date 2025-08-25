# 🐍 Python Development Setup

> **Complete guide to setting up a professional Python development environment on Ubuntu**

## 📋 Table of Contents

- [🚀 Quick Start](#-quick-start)
- [📦 Python Installation](#-python-installation)
- [🔧 Package Management](#-package-management)
- [🏗️ Virtual Environments](#️-virtual-environments)
- [⚙️ Development Tools](#️-development-tools)
- [🎯 Best Practices](#-best-practices)
- [🔍 Troubleshooting](#-troubleshooting)

---

## 🚀 Quick Start

### Essential Setup Commands

```bash path=null start=null
# Update system and install Python essentials
sudo apt update && sudo apt upgrade -y
sudo apt install python3 python3-pip python3.12-venv

# Verify installation
python3 --version
pip3 --version
```

### 5-Minute Development Environment

```bash path=null start=null
# Create project directory
mkdir ~/python-projects && cd ~/python-projects

# Create virtual environment
python3 -m venv myproject

# Activate environment
source myproject/bin/activate

# Install common packages
pip install requests numpy pandas matplotlib

# Start coding!
```

---

## 📦 Python Installation

### System Python

Ubuntu comes with Python pre-installed, but let's ensure you have the latest version and necessary tools.

```bash path=null start=null
# Check current Python version
python3 --version

# Install Python 3 (if not installed)
sudo apt install python3

# Install development headers (needed for some packages)
sudo apt install python3-dev

# Install build essentials (for compiling packages)
sudo apt install build-essential
```

### Alternative Python Versions

#### Using deadsnakes PPA (for latest versions)

```bash path=null start=null
# Add deadsnakes PPA for newer Python versions
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt update

# Install specific Python version (e.g., Python 3.12)
sudo apt install python3.12
sudo apt install python3.12-venv python3.12-dev

# Verify installation
python3.12 --version
```

#### Using pyenv (recommended for multiple versions)

```bash path=null start=null
# Install dependencies
sudo apt install -y make build-essential libssl-dev zlib1g-dev \
libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm \
libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev \
libffi-dev liblzma-dev

# Install pyenv
curl https://pyenv.run | bash

# Add to shell profile
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(pyenv init -)"' >> ~/.bashrc

# Restart shell
exec "$SHELL"

# Install and set Python version
pyenv install 3.12.0
pyenv global 3.12.0
```

---

## 🔧 Package Management

### pip - The Python Package Installer

#### Installation and Upgrade

```bash path=null start=null
# Install pip (if not already installed)
sudo apt install python3-pip

# Upgrade pip to latest version
python3 -m pip install --upgrade pip

# Check pip version
pip3 --version
```

#### Essential pip Commands

```bash path=null start=null
# Install packages
pip install package_name
pip install package_name==1.2.3      # Specific version
pip install 'package_name>=1.2.3'    # Minimum version

# Install from requirements file
pip install -r requirements.txt

# Upgrade packages
pip install --upgrade package_name
pip list --outdated                   # Show outdated packages

# Uninstall packages
pip uninstall package_name

# Show package information
pip show package_name

# List installed packages
pip list
pip freeze                           # Format suitable for requirements.txt
```

#### Creating Requirements Files

```bash path=null start=null
# Generate requirements.txt
pip freeze > requirements.txt

# Install from requirements.txt
pip install -r requirements.txt

# Example requirements.txt content:
# requests==2.28.1
# numpy==1.24.1
# pandas==1.5.2
# matplotlib==3.6.2
```

### pipx - For Installing Python Applications

```bash path=null start=null
# Install pipx
sudo apt install pipx

# Ensure pipx path is in PATH
pipx ensurepath

# Install applications globally (isolated environments)
pipx install black        # Code formatter
pipx install flake8       # Linter
pipx install poetry       # Dependency management
pipx install jupyterlab   # Jupyter Lab
```

---

## 🏗️ Virtual Environments

### Why Use Virtual Environments?

- **Isolation**: Keep project dependencies separate
- **Version Control**: Use different package versions for different projects
- **Clean System**: Don't clutter your system Python installation
- **Reproducibility**: Easy to recreate environments

### venv (Built-in)

#### Creating and Managing Virtual Environments

```bash path=null start=null
# Create virtual environment
python3 -m venv myenv
python3 -m venv /path/to/project/venv    # Custom location

# Activate virtual environment
source myenv/bin/activate

# Deactivate virtual environment
deactivate

# Remove virtual environment
rm -rf myenv
```

#### Advanced venv Usage

```bash path=null start=null
# Create with specific Python version
python3.11 -m venv myenv

# Create with system site packages (not recommended)
python3 -m venv --system-site-packages myenv

# Create without pip (minimal environment)
python3 -m venv --without-pip myenv
```

### Virtual Environment Best Practices

#### Project Structure

```bash path=null start=null
# Recommended project structure
myproject/
├── venv/                 # Virtual environment
├── src/                  # Source code
│   └── myproject/
├── tests/                # Test files
├── docs/                 # Documentation
├── requirements.txt      # Dependencies
├── requirements-dev.txt  # Development dependencies
├── README.md
└── .gitignore
```

#### Environment Automation

Create a project setup script:

```bash path=null start=null
#!/bin/bash
# setup_project.sh

PROJECT_NAME=$1
if [ -z "$PROJECT_NAME" ]; then
    echo "Usage: ./setup_project.sh <project_name>"
    exit 1
fi

# Create project directory
mkdir "$PROJECT_NAME"
cd "$PROJECT_NAME"

# Create virtual environment
python3 -m venv venv

# Activate environment
source venv/bin/activate

# Install essential packages
pip install --upgrade pip
pip install requests pytest black flake8

# Create project structure
mkdir src tests docs
touch README.md requirements.txt .gitignore

# Create basic requirements.txt
pip freeze > requirements.txt

echo "Project $PROJECT_NAME created successfully!"
echo "To activate: cd $PROJECT_NAME && source venv/bin/activate"
```

Make it executable and use:

```bash path=null start=null
chmod +x setup_project.sh
./setup_project.sh myawesomeproject
```

---

## ⚙️ Development Tools

### Code Editors and IDEs

#### Visual Studio Code

```bash path=null start=null
# Install VS Code
sudo apt update
sudo apt install software-properties-common apt-transport-https wget
wget -q https://packages.microsoft.com/keys/microsoft.asc -O- | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/trusted.gpg.d/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main"
sudo apt install code

# Or download .deb package from https://code.visualstudio.com/
```

**Essential VS Code Extensions for Python:**
- Python (Microsoft)
- Pylance (Microsoft)
- Python Docstring Generator
- GitLens
- Jupyter
- Black Formatter

#### PyCharm

```bash path=null start=null
# Install PyCharm Community (free)
sudo snap install pycharm-community --classic

# Or download from JetBrains website
```

### Command Line Tools

#### Essential Development Packages

```bash path=null start=null
# Install development essentials
pip install black          # Code formatter
pip install flake8         # Linter
pip install pylint         # Advanced linter
pip install mypy           # Type checker
pip install pytest         # Testing framework
pip install pytest-cov     # Coverage plugin
pip install ipython        # Enhanced Python shell
pip install jupyter        # Jupyter notebooks
```

#### Code Quality Tools

```bash path=null start=null
# Format code with Black
black my_script.py
black src/                 # Format entire directory

# Lint with flake8
flake8 my_script.py
flake8 src/

# Type checking with mypy
mypy my_script.py

# Run tests with pytest
pytest                     # Run all tests
pytest tests/              # Run tests in directory
pytest -v                  # Verbose output
pytest --cov=src          # Coverage report
```

### Jupyter Notebook Setup

```bash path=null start=null
# Install Jupyter
pip install jupyter

# Install JupyterLab (modern interface)
pip install jupyterlab

# Start Jupyter Notebook
jupyter notebook

# Start JupyterLab
jupyter lab

# Install useful Jupyter extensions
pip install ipywidgets     # Interactive widgets
pip install matplotlib     # Plotting
pip install seaborn        # Statistical plotting
```

---

## 🎯 Best Practices

### Project Organization

```bash path=null start=null
# Use consistent project structure
myproject/
├── venv/                 # Virtual environment (don't commit to git)
├── src/
│   └── myproject/
│       ├── __init__.py
│       ├── main.py
│       └── utils.py
├── tests/
│   ├── __init__.py
│   ├── test_main.py
│   └── test_utils.py
├── docs/
├── requirements.txt
├── requirements-dev.txt
├── setup.py              # If creating a package
├── README.md
├── .gitignore
└── .env.example          # Environment variables template
```

### Dependency Management

```bash path=null start=null
# Pin versions in requirements.txt
requests==2.28.1
numpy==1.24.1

# Use requirements-dev.txt for development dependencies
pytest==7.2.0
black==22.12.0
flake8==6.0.0

# Install development dependencies
pip install -r requirements.txt
pip install -r requirements-dev.txt
```

### Environment Variables

```bash path=null start=null
# Install python-dotenv for environment variable management
pip install python-dotenv

# Create .env file (don't commit to git)
echo "API_KEY=your_secret_key" > .env
echo "DEBUG=True" >> .env

# Use in Python code
from dotenv import load_dotenv
import os

load_dotenv()
api_key = os.getenv('API_KEY')
```

### Git Integration

```bash path=null start=null
# Create .gitignore for Python projects
cat > .gitignore << EOF
# Virtual environments
venv/
env/
ENV/

# Python compiled files
__pycache__/
*.pyc
*.pyo
*.pyd

# Environment variables
.env

# IDE files
.vscode/
.idea/

# OS files
.DS_Store
Thumbs.db

# Distribution / packaging
build/
dist/
*.egg-info/
EOF
```

---

## 🔍 Troubleshooting

### Common Issues and Solutions

#### Virtual Environment Issues

**Problem**: `venv` module not found
```bash path=null start=null
# Solution: Install python3-venv
sudo apt install python3.12-venv  # Replace with your Python version
```

**Problem**: Permission denied when installing packages
```bash path=null start=null
# Solution: Don't use sudo with pip in virtual environments
# Make sure virtual environment is activated
source venv/bin/activate
pip install package_name  # No sudo needed
```

#### Package Installation Issues

**Problem**: Package compilation fails
```bash path=null start=null
# Solution: Install development headers
sudo apt install python3-dev build-essential

# For specific packages, you might need:
sudo apt install libffi-dev          # For cryptography
sudo apt install libxml2-dev         # For lxml
sudo apt install libjpeg-dev         # For Pillow
```

**Problem**: pip is outdated
```bash path=null start=null
# Solution: Upgrade pip
python3 -m pip install --upgrade pip
```

#### Import Issues

**Problem**: Module not found even though installed
```bash path=null start=null
# Check if virtual environment is activated
which python
which pip

# Verify package installation
pip list | grep package_name

# Check Python path
python -c "import sys; print(sys.path)"
```

### Performance Optimization

#### Faster Package Installation

```bash path=null start=null
# Use binary wheels when available
pip install --prefer-binary package_name

# Use multiple parallel downloads
pip install --upgrade pip setuptools wheel

# Cache downloaded packages
pip install --cache-dir ~/.cache/pip package_name
```

#### Memory Management

```bash path=null start=null
# Monitor memory usage
pip install memory_profiler
python -m memory_profiler script.py

# Use lightweight alternatives
pip install uvloop           # Faster event loop
pip install orjson           # Faster JSON processing
```

---

## 🚀 Advanced Topics

### Package Development

```bash path=null start=null
# Create setup.py for your package
cat > setup.py << EOF
from setuptools import setup, find_packages

setup(
    name="myproject",
    version="0.1.0",
    packages=find_packages(where="src"),
    package_dir={"": "src"},
    install_requires=[
        "requests>=2.25.0",
    ],
    python_requires=">=3.8",
)
EOF

# Install your package in development mode
pip install -e .
```

### Poetry (Advanced Dependency Management)

```bash path=null start=null
# Install Poetry
curl -sSL https://install.python-poetry.org | python3 -

# Initialize new project
poetry new myproject
cd myproject

# Add dependencies
poetry add requests numpy

# Add development dependencies
poetry add --group dev pytest black

# Install dependencies
poetry install

# Activate virtual environment
poetry shell

# Run commands in environment
poetry run python script.py
```

### Docker Integration

```dockerfile
# Dockerfile for Python application
FROM python:3.12-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY src/ ./src/

CMD ["python", "-m", "src.main"]
```

---

## 📚 Learning Resources

### Official Documentation
- [Python.org](https://www.python.org/) - Official Python website
- [Python Package Index (PyPI)](https://pypi.org/) - Package repository
- [Virtual Environments Guide](https://docs.python.org/3/tutorial/venv.html)

### Tools and Utilities
- [pip documentation](https://pip.pypa.io/)
- [Black code formatter](https://black.readthedocs.io/)
- [pytest testing framework](https://docs.pytest.org/)

---

> 💡 **Pro Tip**: Always use virtual environments for Python projects. It's the difference between a messy system and a clean, manageable development setup!

**Next Steps**: Ready to dive deeper? Check out [Advanced Development Setup](dev-environment.md) or explore [Version Control Integration](version-control.md).
