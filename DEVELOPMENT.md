# Development Setup Guide

This guide helps you set up a local development environment for the vortex-sdk-python package.

## Python Installation with Homebrew (macOS)

If you installed Python with Homebrew, you may encounter permission issues with system-wide installations. The solution is to use a virtual environment.

## Setting Up Virtual Environment

### 1. Create a Virtual Environment

```bash
# Navigate to project directory
cd vortex-python-sdk

# Create virtual environment using Homebrew Python
python3 -m venv venv

# Alternative: specify explicit Python version
/opt/homebrew/bin/python3 -m venv venv
```

### 2. Activate Virtual Environment

```bash
# On macOS/Linux
source venv/bin/activate

# On Windows
venv\Scripts\activate
```

After activation, your terminal prompt should show `(venv)` prefix.

### 3. Upgrade pip in Virtual Environment

```bash
pip install --upgrade pip setuptools wheel
```

### 4. Install the Package in Development Mode

```bash
# Install package with all development dependencies
pip install -e ".[dev]"
```

### 5. Install Node.js Dependencies

```bash
# Install npm dependencies (required for the SDK)
npm install
```

### 6. Verify Installation

```bash
# Test import
python -c "from vortex_sdk import VortexSDK; print('✓ Import successful')"

# Run tests
pytest

# Check Node.js SDK is accessible
node -e "require.resolve('@vortexfi/sdk')"
```

## Complete Setup Commands (Copy-Paste)

```bash
# 1. Create and activate venv
python3 -m venv venv
source venv/bin/activate

# 2. Upgrade pip
pip install --upgrade pip setuptools wheel

# 3. Install Node.js dependencies
npm install

# 4. Install Python package in development mode
pip install -e ".[dev]"

# 5. Install pre-commit hooks (optional)
pre-commit install

# 6. Verify everything works
python -c "from vortex_sdk import VortexSDK; print('✓ Ready!')"
pytest
```

## Deactivating Virtual Environment

When you're done working:

```bash
deactivate
```

## Troubleshooting

### "Permission denied" or "externally-managed-environment"

**Problem:** Cannot install packages due to system protection.

**Solution:** Always use a virtual environment (venv). Never use `sudo pip`.

```bash
# Create venv if you haven't
python3 -m venv venv
source venv/bin/activate
pip install -e ".[dev]"
```

### "Command not found: python3"

**Problem:** Python not in PATH or not installed.

**Solution:**

```bash
# Install Python via Homebrew
brew install python@3.11

# Use explicit path
/opt/homebrew/bin/python3 -m venv venv
```

### "Node.js not found"

**Problem:** Node.js not installed.

**Solution:**

```bash
# Install Node.js via Homebrew
brew install node

# Verify installation
node --version
npm --version
```

### "@vortexfi/sdk not found"

**Problem:** npm dependencies not installed.

**Solution:**

```bash
# Install npm dependencies
npm install

# Or install globally
npm install -g @vortexfi/sdk
```

### Virtual environment not activating

**Problem:** `source venv/bin/activate` doesn't work.

**Solution:**

```bash
# Try with dot notation
. venv/bin/activate

# Or use full path
source /Users/yourname/vortex-python-sdk/venv/bin/activate
```

### "No module named 'vortex_sdk'"

**Problem:** Package not installed in development mode.

**Solution:**

```bash
# Make sure venv is activated
source venv/bin/activate

# Install in development mode
pip install -e .
```

## Working with Multiple Python Versions

If you need to test with different Python versions:

```bash
# Create venv with specific Python version
python3.9 -m venv venv39
python3.10 -m venv venv310
python3.11 -m venv venv311

# Activate the one you want
source venv39/bin/activate
pip install -e ".[dev]"
pytest
```

## IDE Setup

### VSCode

1. Install Python extension
2. Select interpreter: `Cmd+Shift+P` → "Python: Select Interpreter"
3. Choose `./venv/bin/python`

Add to `.vscode/settings.json`:
```json
{
  "python.defaultInterpreterPath": "${workspaceFolder}/venv/bin/python",
  "python.terminal.activateEnvironment": true,
  "python.formatting.provider": "black",
  "python.linting.enabled": true,
  "python.linting.ruffEnabled": true
}
```

### PyCharm

1. File → Settings → Project → Python Interpreter
2. Click gear icon → Add
3. Select "Existing environment"
4. Choose `venv/bin/python`

## Daily Development Workflow

```bash
# 1. Activate venv
source venv/bin/activate

# 2. Make your changes
# ... edit code ...

# 3. Run tests
pytest

# 4. Format code
black src/ tests/ examples/

# 5. Check linting
ruff check src/ tests/ examples/

# 6. Type check
mypy src/

# 7. When done
deactivate
```

## Running Examples

```bash
# Activate venv
source venv/bin/activate

# Run example
python examples/brl_onramp_example.py
```

## Updating Dependencies

```bash
# Activate venv
source venv/bin/activate

# Update Python dependencies
pip install --upgrade -e ".[dev]"

# Update Node.js dependencies
npm update
```

## Clean Start

If you encounter issues, start fresh:

```bash
# Remove virtual environment
rm -rf venv

# Remove Python build artifacts
rm -rf build/ dist/ *.egg-info src/*.egg-info

# Remove Node.js dependencies
rm -rf node_modules package-lock.json

# Start over
python3 -m venv venv
source venv/bin/activate
pip install --upgrade pip setuptools wheel
npm install
pip install -e ".[dev]"
```

## Best Practices

1. **Always use venv** - Never install packages globally or with sudo
2. **Activate before working** - Always activate venv before development
3. **Keep it updated** - Regularly update pip and dependencies
4. **Test in clean environment** - Occasionally test in a fresh venv
5. **Don't commit venv** - The `venv/` directory is in `.gitignore`

## Additional Resources

- [Python Virtual Environments](https://docs.python.org/3/tutorial/venv.html)
- [Homebrew Python Guide](https://docs.brew.sh/Homebrew-and-Python)
- [pip User Guide](https://pip.pypa.io/en/stable/user_guide/)
