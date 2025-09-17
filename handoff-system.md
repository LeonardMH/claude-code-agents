# Agent Handoff System

## Overview
All agents communicate through a shared workspace at `.agent-handoffs/` using structured markdown files to pass context, findings, and recommendations between agents.

## File Naming Convention
Format: `<agent-role-name>-<short-uuid>.md`

Examples:
- `requirements-analyst-a1b2.md`
- `code-implementer-c3d4.md`
- `runtime-debugger-e5f6.md`

## Agent Workflow

### 1. requirements-analyst
- **Creates**: Project requirements and analysis
- **Handoff to**: code-implementer
- **File includes**: Requirements document, critical decisions, open questions

### 2. code-implementer
- **Reads**: requirements-analyst handoffs
- **Creates**: Implementation code and documentation
- **Handoff to**: code-reviewer, build-error-analyzer
- **File includes**: Architecture decisions, key components, testing needs

### 3. runtime-debugger
- **Creates**: Bug analysis and root cause findings
- **Handoff to**: code-implementer (for fixes)
- **File includes**: Issue analysis, evidence, reproduction steps, fix recommendations

### 4. build-error-analyzer
- **Creates**: Build error analysis and fixes
- **Handoff to**: code-implementer (for fixes)
- **File includes**: Build config, critical errors, specific fix recommendations

### 5. code-reviewer
- **Reads**: code-implementer handoffs
- **Creates**: Quality assessment and improvement suggestions
- **Handoff to**: code-implementer (for improvements)
- **File includes**: Quality assessment, refactoring suggestions, priority levels

### 6. git-archaeologist
- **Creates**: Historical code findings
- **Handoff to**: Any agent needing historical context
- **File includes**: Historical code, commit references, evolution patterns

## Handoff File Structure

Each handoff file should contain:

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
- Always check for relevant handoffs before starting work
- Create handoff files after completing significant analysis or implementation
- Use descriptive UUIDs or short identifiers for easy reference
- Keep handoffs concise but comprehensive
- Reference specific files:line_numbers when applicable