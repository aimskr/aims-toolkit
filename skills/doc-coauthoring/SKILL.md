---
name: doc-coauthoring
description: "문서 작성, 문서화, 문서, 스펙 작성, 기술 문서, 제안서 - Guide users through a structured workflow for co-authoring documentation. Use when user wants to write documentation, proposals, technical specs, decision docs, or similar structured content."
allowed-tools: Read, Write, Edit, Grep, Glob
---

# Doc Co-Authoring Workflow

Guide users through collaborative document creation via three stages.

## When to Offer This Workflow

**Trigger conditions:**
- User mentions writing documentation: "write a doc", "draft a proposal", "create a spec"
- User mentions specific doc types: "PRD", "design doc", "decision doc", "RFC"
- User seems to be starting a substantial writing task

## The Three Stages

### Stage 1: Context Gathering
- Ask meta-context questions (doc type, audience, desired impact)
- Encourage info dumping (background, discussions, constraints)
- Ask 5-10 clarifying questions to close knowledge gaps
- **Exit when:** Edge cases and trade-offs can be discussed without needing basics

### Stage 2: Refinement & Structure
For each section:
1. Ask clarifying questions
2. Brainstorm 5-20 options
3. User curates (keep/remove/combine)
4. Draft the section
5. Iterative refinement via `str_replace`

**Section order:** Start with most unknowns, leave summary for last

### Stage 3: Reader Testing
- Predict 5-10 reader questions
- Test with fresh Claude (sub-agent or new session)
- Check for ambiguity, assumptions, contradictions
- Fix any gaps found

## Key Principles

- **One question at a time** - Don't overwhelm
- **Surgical edits** - Use `str_replace`, never reprint whole doc
- **Quality over speed** - Each iteration should improve meaningfully
- **User agency** - Let them skip or adjust the process

## Detailed Reference

For complete stage instructions, tips, and handling edge cases:
**Read `REFERENCE.md` in this skill directory when needed.**
