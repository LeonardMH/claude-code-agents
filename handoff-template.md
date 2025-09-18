# Agent Handoff Template

Use this template structure when creating handoff files to ensure consistency across all agents.

## File Naming Convention
Format: `<agent-role-name>-<short-uuid>.md`

Examples:
- `plan-requirements-a1b2c3.md`
- `build-code-a2b4c8.md`
- `fix-runtime-e1c7g6.md`

## Template Structure

```markdown
# Agent Handoff: [Agent Name]
Generated: [timestamp]
Task: [brief description]

## Summary
[Brief overview of work completed]

## Key Findings
[Important discoveries or results]

## Context for Next Agent
[What the next agent needs to know]

## Recommendations
[Specific actions or considerations]

## Files/Components Affected
[Relevant code locations or artifacts]
```

## Usage Guidelines
- Always check this template before writing handoff files
- Keep handoffs concise but comprehensive
- Reference specific files:line_numbers when applicable
- Use descriptive UUIDs or short identifiers for easy reference