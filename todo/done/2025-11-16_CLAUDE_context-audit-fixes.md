# Context Audit Fixes

**Date:** 2025-11-16
**Version:** 1.0
**Author:** uweli
**Category:** CLAUDE

## Executive Summary

Todo list for addressing issues identified in the 2025-11-16 project context audit. Contains 8 major issues (high priority), 4 medium priority items, and 4 low priority cosmetic fixes.

<br><br>

## High Priority Tasks (Major Issues)

### Enhance .claudeignore Coverage
- [x] Add `.git/` to .claudeignore
- [x] Add `*.egg-info/` to .claudeignore
- [x] Add `.coverage` to .claudeignore (if not already present)

### Update CLAUDE.md Dependency Installation
- [x] Replace references to `requirements.txt` with `pyproject.toml` installation method
- [x] Update line 129 to use `pip install -e .` or `pip install -e ".[dev]"`
- [x] Remove outdated requirements.txt content block at lines 275-288

### Address main.py References
- [x] Decide: implement main.py or mark as planned future work
- [x] If planned: Add status indicator in CLAUDE.md Development Commands section (lines 134-146)
- [x] If planned: Update directory structure section (line 85)

### Fix Cross-References in Specs Files
- [x] Update `specs/PROJECT_OVERVIEW.md` lines 111-112 to point to `../.claude/guides/`
- [x] Update `specs/ARCHITECTURE.md` lines 33-34 to show `.claude/guides/` instead of `docs/`

### Update README.md Directory Structure
- [x] Fix lines 41-45 to reflect actual `src/`-based architecture
- [x] Remove references to non-existent `metrics/`, `ai_analysis/`, `visualization/`, `evaluation/` directories

### Fix project-context-auditor.md Path References
- [x] Update line 83 to use `.claude/audit/yyyy-mm-dd_context-audit-report.md`
- [x] Update line 92 to use `.claude/audit/claude_audit.csv`

<br><br>

## Medium Priority Tasks

### Update CLAUDE.md Metadata
- [x] Change date from 2025-11-15 to 2025-11-16

### Sync pyproject.toml Dependencies
- [x] Remove `plotly` and `openai` references from CLAUDE.md (install when needed)

### Update GUIDE_TODO.md Categories
- [x] Add `SETUP` and `CLAUDE` to categories list (line 22)
- [x] Update examples to include these new categories

<br><br>

## Low Priority Tasks (Cosmetic)

### Fix Typos
- [x] Fix "Recommendet" → "Recommended" in GUIDE_PROJECT.md line 28
- [x] Fix "seperation" → "separation" in GUIDE_DOCS.md line 49

### Add Missing Table of Contents
- [x] Add ToC to README.md (5 sections)
- [x] Consider restructuring GUIDE_PROJECT.md to follow standard format (already compliant)

### Add Missing Section Separators
- [x] Add `<br><br>` after final section in GUIDE_DOCS.md
- [x] Add `<br><br>` after final section in GUIDE_PROJECT.md
- [x] Add `<br><br>` after final section in GUIDE_TODO.md

<br><br>

## Notes

- Source: `.claude/audit/2025-11-16_context-audit-report.md`
- Total issues: 8 major, 12 minor
- Compliance scores: GUIDE_PROJECT.md 75%, GUIDE_DOCS.md 70%, .claudeignore 90%
- Priority order follows audit report recommendations
