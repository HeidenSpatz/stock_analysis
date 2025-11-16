# Project Setup

**Date:** 2025-11-16
**Version:** 1.0
**Author:** uweli

## Executive Summary

Basic project setup tasks for the stock analysis tool, focusing on directory structure, Python environment, dependencies, and testing infrastructure.

<br><br>

## Directory Structure

- [x] Create project directories (src/, data/, outputs/, templates/, config/)
- [x] Create documentation directories (docs/, requirements/, specs/)
- [x] Create organizational directories (.claude/, todo/, archive/)
- [x] Set up .gitignore for Python and project-specific exclusions
- [x] Set up .claudeignore for token efficiency

<br><br>

## Documentation

- [x] Create requirements/REQUIREMENTS.md
- [x] Create specs/PROJECT_SPECIFICATION.md
- [x] Create specs/PROJECT_OVERVIEW.md
- [x] Create specs/ARCHITECTURE.md
- [x] Create specs/MODULE_*.md for all 5 modules
- [x] Create specs/DEVELOPMENT_GUIDE.md
- [x] Create specs/CONFIGURATION.md
- [x] Create .claude/CLAUDE.md with project instructions
- [x] Create .claude/guides/GUIDE_DOCS.md
- [x] Create .claude/guides/GUIDE_TODO.md

<br><br>

## Python Environment

- [x] Install python3-venv package (sudo apt install python3.12-venv)
- [x] Verify venv installation (python3 -m venv --help)
- [x] Create virtual environment (python3 -m venv .venv)
- [x] Activate virtual environment (source .venv/bin/activate)
- [x] Verify Python version (which python && python --version) - Python 3.12.3
- [x] Upgrade pip to latest version (pip install --upgrade pip)
- [x] Verify pip version (pip --version) - pip 25.3

<br><br>

## VS Code Editor

- [x] Install VS Code on Windows 11 (download from code.visualstudio.com)
- [x] Install WSL extension in VS Code (ms-vscode-remote.remote-wsl)
- [x] Open WSL terminal tab in Windows Terminal
- [x] Navigate to project directory (cd /home/uweli/projects/stock_analysis)
- [x] Activate virtual environment (source .venv/bin/activate)
- [x] Launch VS Code from WSL (code .)
- [x] Install Python extension when prompted (ms-python.python)
- [x] Verify VS Code detects .venv interpreter (open a .py file, check bottom-right status bar shows "3.12.3 (.venv)" - note: only visible when .py file is active, not for .md files)

<br><br>

## Dependencies

- [x] Create requirements.txt with core dependencies (cat > requirements.txt - contains: yfinance, pandas, numpy, openpyxl, matplotlib, pandas-ta, anthropic, pyyaml, requests, python-dotenv)
- [x] Create requirements-dev.txt for development tools (cat > requirements-dev.txt - contains: pytest, pytest-cov, black, flake8)
- [x] Install dependencies from requirements.txt (pip install -r requirements.txt)
- [x] Install development dependencies from requirements-dev.txt (pip install -r requirements-dev.txt)
- [x] Verify all imports work (python -c "import yfinance, pandas, numpy, openpyxl, matplotlib, anthropic, yaml, dotenv; print('All imports successful')")

<br><br>

## Testing Infrastructure

- [x] Create tests/ directory (mkdir tests)
- [ ] Create tests/conftest.py for pytest configuration (cat > tests/conftest.py)
- [ ] Create tests/__init__.py
- [ ] Add pytest.ini or pyproject.toml with pytest settings
- [ ] Run pytest to verify setup (should show 0 tests collected)
- [ ] Create .coveragerc for coverage configuration

<br><br>

## Source Code Structure

- [ ] Create src/__init__.py
- [ ] Create placeholder files for all 5 modules:
  - [ ] src/data_acquisition.py
  - [ ] src/metrics_calculator.py
  - [ ] src/ai_analyzer.py
  - [ ] src/visualizer.py
  - [ ] src/evaluator.py
- [ ] Create main.py orchestrator script skeleton

<br><br>

## Configuration (Deferred)

Configuration setup is intentionally deferred to a separate todo. This includes:
- config/config.yaml
- config/analysis_profiles.yaml
- Environment variables and secrets management

Requires deeper understanding of configuration needs before implementation.

<br><br>

## Verification

- [ ] Run `python --version` to confirm Python 3.9+
- [ ] Run `pip list` to verify all dependencies installed
- [ ] Run `pytest --version` to verify pytest installed
- [ ] Run `git status` to confirm all setup files are tracked or ignored appropriately
- [ ] Import all planned libraries in Python to catch any installation issues
