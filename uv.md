# UV - Python Package Manager

## 1. Initializing a Project
### Navigate to your project directory and initialize UV
```
cd your-project-directory
uv init
```

## 2. Installing Dependencies
### 2.1 Install production dependencies
```
uv sync
```
### 2.2 Install with dev dependencies
```
uv sync --extra dev
```
### 2.3 Install only production dependencies
```
uv sync --no-dev
```

## 3. Adding Dependencies
### 3.1 Add new production dependency
```
uv add dependency-name
```
### 3.2 Add dev dependency
```
uv add --dev dependency-name
```

## 4. Removing Dependencies
```
uv remove package
```

## 5. Pin Dependencies (Create Lockfile)
```
uv lock
```

## 6. Running Your Code
### 6.1 Activate virtual environment
```
uv shell
```
### 6.2 Run code directly without activating environment
```
uv run file_name.py
```

## 7. Creating Virtual Environment
```
uv venv
```

## 8. Migration from requirements.txt
### 8.1 Add dependencies from requirements.txt
```
uv add -r requirements.txt
```
### 8.2 Remove old requirements.txt file
```
rm requirements.txt
```
### 8.3 Create lockfile
```
uv lock
```

## 9. Best Practices
### 9.1 Always commit both files
```
pyproject.toml and uv.lock
```
### 9.2 Use uv sync for installation
```
Don't use pip install
```
### 9.3 Pin major versions
```
package>=1.0.0,<2.0.0
```
### 9.4 Separate dev dependencies
```
Use --dev flag
```
### 9.5 Regular updates
```
Run uv lock --upgrade monthly
```
### 9.6 Use uv run
```
Instead of activating venv manually
```