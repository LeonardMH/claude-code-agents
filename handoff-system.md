# Agent Handoff System

## Overview
All agents communicate through a shared workspace at `.agent-handoffs/` using structured markdown files to pass context, findings, and recommendations between agents.

## File Naming Convention
Format: `<agent-role-name>-<short-uuid>.md`

Examples:
- `requirements-analyst-a1b2c3.md`
- `code-implementer-a2b4c8.md`
- `runtime-debugger-e1c7g6.md`

## Agent Workflow

### 1. requirements-analyst
- **Creates**: Project requirements and analysis
- **Handoff to**: api-designer, code-implementer
- **File includes**: Requirements document, critical decisions, open questions

### 2. api-designer
- **Reads**: requirements-analyst handoffs (when designing from requirements)
- **Creates**: API specifications and interface contracts
- **Handoff to**: code-implementer
- **File includes**: Interface definitions, data schemas, usage patterns, evolution strategy

### 3. code-implementer
- **Reads**: requirements-analyst, api-designer handoffs
- **Creates**: Implementation code and documentation
- **Handoff to**: code-reviewer, build-error-analyzer
- **File includes**: Architecture decisions, key components, testing needs

### 4. runtime-debugger
- **Creates**: Bug analysis and root cause findings
- **Handoff to**: code-implementer (for fixes)
- **File includes**: Issue analysis, evidence, reproduction steps, fix recommendations

### 5. build-error-analyzer
- **Creates**: Build error analysis and fixes
- **Handoff to**: code-implementer (for fixes)
- **File includes**: Build config, critical errors, specific fix recommendations

### 6. code-reviewer
- **Reads**: code-implementer handoffs
- **Creates**: Quality assessment and improvement suggestions
- **Handoff to**: code-implementer (for improvements)
- **File includes**: Quality assessment, refactoring suggestions, priority levels

### 7. code-explorer
- **Creates**: Codebase structural analysis and architectural overview
- **Handoff to**: requirements-analyst (system context), code-implementer (conventions), documentation-writer (architecture)
- **File includes**: Project structure overview, module inventory, technology stack, architectural patterns, key APIs

### 8. git-archaeologist
- **Creates**: Historical code findings
- **Handoff to**: Any agent needing historical context
- **File includes**: Historical code, commit references, evolution patterns

### 9. test-specialist
- **Reads**: code-implementer handoffs
- **Creates**: Test suites and execution results
- **Handoff to**: runtime-debugger (for test failures)
- **File includes**: Test coverage analysis, failing test details, testing strategy

### 10. security-auditor
- **Reads**: code-implementer handoffs
- **Creates**: Security vulnerability assessments and compliance reports
- **Handoff to**: code-implementer (for fixes)
- **File includes**: Vulnerability assessment, remediation priorities, compliance gaps

### 11. performance-profiler
- **Creates**: Performance analysis and optimization recommendations
- **Handoff to**: code-implementer (for optimizations)
- **File includes**: Performance metrics, bottleneck analysis, optimization strategies

### 12. documentation-writer
- **Reads**: All agent handoffs for comprehensive project context
- **Creates**: Documentation summaries and maintenance guides
- **Handoff to**: Final documentation deliverables
- **File includes**: Documentation structure, key content areas, maintenance recommendations

### 13. spreadsheet-wizard
- **Creates**: Spreadsheet analysis and data extraction results
- **Handoff to**: requirements-analyst (data context), code-implementer (data integration), documentation-writer (data documentation)
- **File includes**: Data structure summary, quality issues found, extraction tools created, recommendations for data handling

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