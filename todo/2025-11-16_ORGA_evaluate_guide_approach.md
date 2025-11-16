# Evaluate Guide Approach vs Alternatives

**Date:** 2025-11-16
**Version:** 1.0
**Author:** uweli

## Executive Summary

Todo list to evaluate whether the current guide-based approach (.claude/guides/) is optimal, or if slash commands or subagents would be more effective for standardizing documentation and todo creation.

<br><br>

## Tasks

- [ ] Test current guide approach: create 2-3 documentation files using GUIDE_DOCS.md and assess consistency
- [ ] Test current guide approach: create 2-3 todo files using GUIDE_TODO.md and assess naming/structure adherence
- [ ] Research Claude Code slash commands: review documentation at code.claude.com for custom command capabilities
- [ ] Research Claude Code subagents: check if specialized agents can be configured for doc/todo generation
- [ ] Compare token efficiency: measure context size when using guides vs potential slash command approach
- [ ] Compare user experience: assess ease of triggering (verbal request vs /command vs agent invocation)
- [ ] Evaluate consistency: determine which approach produces most reliable adherence to standards
- [ ] Make decision: document chosen approach and rationale in docs/decisions/ (if creating ADR log)
- [ ] If switching approaches: migrate from guides to slash commands or subagents
- [ ] If keeping guides: validate and refine GUIDE_DOCS.md and GUIDE_TODO.md based on testing

<br><br>

## Evaluation Criteria

**Guide-based approach:**
- Pros: Simple, already in CLAUDE.md context, easy to update
- Cons: Relies on Claude reading and following instructions, no enforcement

**Slash commands:**
- Pros: Explicit trigger, can template boilerplate, consistent invocation
- Cons: Requires command file creation, less flexible than free-form

**Subagents:**
- Pros: Specialized behavior, can have dedicated tools/context
- Cons: May be overkill for simple doc/todo creation, setup complexity

<br><br>

## Notes

Defer this evaluation until after creating 3-5 files using current approach to gather real usage data.
