# Optimize Context Auditor Agent

**Date:** 2025-11-16
**Version:** 1.0
**Author:** uweli
**Category:** CLAUDE

## Executive Summary

Todo list for analyzing and optimizing the project-context-auditor agent to improve efficiency and reduce token consumption through multi-stage auditing approach.

<br><br>

## Tasks

### Analyze Current Agent Implementation
- [ ] Review current project-context-auditor agent configuration and methodology
- [ ] Identify token consumption patterns and inefficiencies
- [ ] Document current audit scope and file reading approach

### Design Optimized Multi-Stage Audit Strategy
- [ ] Design Stage 1: Lightweight folder tree analysis and Claude-specific file validation
  - Check directory structure matches documentation
  - Verify .claude/ references (agents, guides, audit files)
  - Validate .claudeignore patterns
  - Check for missing critical Claude files
  - No deep file content reading in this stage
- [ ] Design Stage 2: Deep content analysis (separate agent or conditional execution)
  - Cross-reference validation within files
  - Compliance checks against GUIDE_DOCS.md and GUIDE_PROJECT.md
  - Detailed line-by-line reference validation
  - Only run if Stage 1 finds issues or on-demand

### Implementation Options
- [ ] Option A: Split into two separate agents (context-auditor-quick, context-auditor-deep)
- [ ] Option B: Single agent with mode parameter (--quick or --deep)
- [ ] Option C: Chain two agents where Stage 1 triggers Stage 2 conditionally
- [ ] Evaluate pros/cons of each approach considering token efficiency

### Implement Optimization
- [ ] Update agent configuration with chosen approach
- [ ] Add lightweight tree-based validation logic
- [ ] Separate deep analysis into conditional/secondary stage
- [ ] Update audit report format to reflect multi-stage results

### Testing and Validation
- [ ] Test optimized agent on current project state
- [ ] Compare token usage: old vs new approach
- [ ] Verify all issues from 2025-11-16 audit would still be caught
- [ ] Document expected token savings

<br><br>

## Notes

- Current audit used ~39000 tokens for comprehensive analysis
- Goal: Reduce routine audits to <5000 tokens with Stage 1 only
- Stage 2 deep analysis only when needed or explicitly requested
- Consider: Can folder tree + .claude/ file checks catch 80% of issues at 10% cost?

<br><br>

## Success Criteria

- Routine audits complete in <5000 tokens
- All major issues still detectable
- Clear separation between quick validation and deep analysis
- Agent configuration well-documented and maintainable
