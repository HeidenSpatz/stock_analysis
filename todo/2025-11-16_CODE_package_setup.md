# Python Package Setup

**Date:** 2025-11-16
**Version:** 1.0
**Author:** uweli

## Executive Summary

Configure the stock_analysis project as a proper Python package to enable clean imports without path manipulation, using pyproject.toml for modern Python packaging standards.

<br><br>

## Package Configuration

- [x] Create pyproject.toml in project root (cat > pyproject.toml)
- [x] Configure project metadata (name, version, description, authors)
- [x] Specify Python version requirement (>=3.9)
- [x] Move dependencies from requirements.txt to pyproject.toml [project.dependencies]
- [x] Move dev dependencies from requirements-dev.txt to pyproject.toml [project.optional-dependencies.dev]
- [x] Configure package discovery (find packages in src/)

<br><br>

## Package Installation

- [x] Install package in editable mode (pip install -e ".[dev]")
- [x] Verify package installed (pip list | grep stock)
- [x] Create src/__init__.py placeholder
- [x] Create src/data_acquisition.py placeholder for testing
- [x] Test imports work without path manipulation (python -c "from src.data_acquisition import *")

<br><br>

## Cleanup

- [ ] Remove path manipulation from tests/conftest.py
- [ ] Update tests/conftest.py to use direct imports
- [ ] Verify pytest still works (pytest --collect-only)
- [ ] Consider deprecating requirements.txt/requirements-dev.txt (optional - can keep for compatibility)

<br><br>

## Benefits

**Before (with path manipulation):**
```python
# tests/conftest.py
import sys
from pathlib import Path
project_root = Path(__file__).parent.parent
sys.path.insert(0, str(project_root / "src"))
```

**After (clean imports):**
```python
# tests/test_metrics.py
from src.metrics_calculator import calculate_pe_ratio  # Just works!
```

**Why this is better:**
- No path manipulation needed
- Works consistently across different environments
- Standard Python packaging approach
- Easier dependency management
- Enables installation in other projects if needed
