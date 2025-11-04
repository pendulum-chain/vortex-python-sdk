# Publishing to PyPI

This guide explains how to publish the vortex-sdk-python package to the Python Package Index (PyPI).

## Prerequisites

### 1. Create PyPI Accounts
- Create an account on [PyPI](https://pypi.org/account/register/)
- Create an account on [TestPyPI](https://test.pypi.org/account/register/) (for testing)

### 2. Install Build Tools
```bash
pip install --upgrade build twine
```

### 3. Configure API Tokens

#### For PyPI:
1. Go to https://pypi.org/manage/account/token/
2. Create a new API token with "Entire account" scope
3. Save the token (starts with `pypi-`)

#### For TestPyPI:
1. Go to https://test.pypi.org/manage/account/token/
2. Create a new API token
3. Save the token

Create a `~/.pypirc` file:
```ini
[distutils]
index-servers =
    pypi
    testpypi

[pypi]
username = __token__
password = pypi-YOUR_PYPI_TOKEN_HERE

[testpypi]
repository = https://test.pypi.org/legacy/
username = __token__
password = pypi-YOUR_TESTPYPI_TOKEN_HERE
```

Set permissions: `chmod 600 ~/.pypirc`

## Pre-Publishing Checklist

### 1. Update Version
Edit version in both files:
- `setup.py` (line 8)
- `pyproject.toml` (line 7)
- `src/vortex_sdk/__init__.py` (line 21)

Use semantic versioning (e.g., `0.1.0`, `0.2.0`, `1.0.0`)

### 2. Update Documentation
Ensure these files are current:
- `README.md` - Clear installation and usage instructions
- `CHANGELOG.md` - Document changes (create if doesn't exist)

### 3. Clean Build Artifacts
```bash
rm -rf build/ dist/ *.egg-info src/*.egg-info
```

## Building the Package

### 1. Build Distribution Files
```bash
python3 -m build
```

This creates:
- `dist/vortex_sdk_python-X.Y.Z-py3-none-any.whl` (wheel)
- `dist/vortex-sdk-python-X.Y.Z.tar.gz` (source distribution)

### 2. Verify Build
```bash
ls -lh dist/
```

You should see both `.whl` and `.tar.gz` files.

### 3. Check Package
```bash
twine check dist/*
```

All checks should pass.

## Publishing to TestPyPI (Recommended First)

### 1. Upload to TestPyPI
```bash
twine upload --repository testpypi dist/*
```

### 2. Test Installation
```bash
# In a fresh virtual environment
python3 -m venv test_env
source test_env/bin/activate

pip install --index-url https://test.pypi.org/simple/ --extra-index-url https://pypi.org/simple/ vortex-sdk-python

# Test import
python3 -c "from vortex_sdk import VortexSDK; print('✓ Success')"

deactivate
rm -rf test_env
```

## Publishing to PyPI (Production)

### 1. Upload to PyPI
```bash
twine upload dist/*
```

### 2. Verify on PyPI
Visit: https://pypi.org/project/vortex-sdk-python/

### 3. Test Installation
```bash
pip install vortex-sdk-python
python3 -c "from vortex_sdk import VortexSDK; print('✓ Success')"
```

## Post-Publishing

### 1. Create Git Tag
```bash
git tag -a v0.1.0 -m "Release version 0.1.0"
git push origin v0.1.0
```

### 2. Create GitHub Release
1. Go to your repository on GitHub
2. Click "Releases" → "Create a new release"
3. Select the tag you just created
4. Add release notes
5. Publish release

### 3. Update README Badge
Add to README.md:
```markdown
[![PyPI version](https://badge.fury.io/py/vortex-sdk-python.svg)](https://badge.fury.io/py/vortex-sdk-python)
```

## Troubleshooting

### "Invalid distribution filename"
- Ensure version numbers match in `setup.py`, `pyproject.toml`, and `__init__.py`
- Clean build artifacts: `rm -rf build/ dist/ *.egg-info`

### "Upload failed (403 Forbidden)"
- Check API token is correct in `~/.pypirc`
- Ensure token has proper permissions

### "File already exists"
- You cannot re-upload the same version
- Increment version number and rebuild

### "Package name already taken"
- The name `vortex-sdk-python` might be taken
- Consider alternative names like `vortexfi-python` or `vortex-python-sdk`

## Publishing Updates

For subsequent releases:

1. Make your code changes
2. Update version numbers in all three files
3. Update CHANGELOG.md
4. Clean old builds: `rm -rf dist/ build/ *.egg-info`
5. Build: `python3 -m build`
6. Check: `twine check dist/*`
7. Upload: `twine upload dist/*`
8. Tag and release on GitHub

## Important Notes

- **Never delete published versions** - PyPI doesn't allow re-uploading
- **Test on TestPyPI first** - Always test before publishing to production
- **Use semantic versioning** - Major.Minor.Patch (e.g., 1.2.3)
- **Keep credentials safe** - Never commit `~/.pypirc` or API tokens
- **Update docs** - Keep README and examples current

## Additional Resources

- [Python Packaging User Guide](https://packaging.python.org/)
- [PyPI Help](https://pypi.org/help/)
- [Semantic Versioning](https://semver.org/)
