# Agent Handoff System

## Overview
All agents communicate through a shared workspace at `.agent-handoffs/` using structured markdown files to pass context, findings, and recommendations between agents.

## File Naming Convention
Format: `<agent-role-name>-<short-uuid>.md`

Examples:
- `plan-requirements-a1b2c3.md`
- `build-code-a2b4c8.md`
- `fix-runtime-e1c7g6.md`

## Agent Workflow

### plan-requirements
- **Creates**: Project requirements and analysis
- **Handoff to**: plan-api, build-code
- **File includes**: Requirements document, critical decisions, open questions

### plan-api
- **Reads**: plan-requirements handoffs (when designing from requirements)
- **Creates**: API specifications and interface contracts
- **Handoff to**: build-code
- **File includes**: Interface definitions, data schemas, usage patterns, evolution strategy

### build-cli
- **Reads**: plan-requirements, plan-api handoffs
- **Creates**: CLI design specifications
- **Handoff to**: build-code, learn-docs
- **File includes**: Command structure, argument schemas, config formats, help templates

### build-gui
- **Reads**: plan-requirements, plan-api handoffs
- **Creates**: GUI architecture specifications and design systems
- **Handoff to**: build-code, check-tests, learn-docs
- **File includes**: Component hierarchy, state management patterns, accessibility guidelines, responsive design strategies, framework recommendations

### build-code
- **Reads**: plan-requirements, plan-api, build-cli, build-gui handoffs
- **Creates**: Implementation code and documentation
- **Handoff to**: check-quality, fix-build
- **File includes**: Architecture decisions, key components, testing needs

### fix-runtime
- **Creates**: Bug analysis and root cause findings
- **Handoff to**: build-code (for fixes)
- **File includes**: Issue analysis, evidence, reproduction steps, fix recommendations

### fix-build
- **Creates**: Build error analysis and fixes
- **Handoff to**: build-code (for fixes)
- **File includes**: Build config, critical errors, specific fix recommendations

### check-quality
- **Reads**: build-code handoffs
- **Creates**: Quality assessment and improvement suggestions
- **Handoff to**: build-code (for improvements)
- **File includes**: Quality assessment, refactoring suggestions, priority levels

### learn-codebase
- **Creates**: Codebase structural analysis and architectural overview
- **Handoff to**: plan-requirements (system context), build-code (conventions), learn-docs (architecture)
- **File includes**: Project structure overview, module inventory, technology stack, architectural patterns, key APIs

### learn-commits
- **Creates**: Historical code findings
- **Handoff to**: Any agent needing historical context
- **File includes**: Historical code, commit references, evolution patterns

### check-tests
- **Reads**: build-code handoffs
- **Creates**: Test suites and execution results
- **Handoff to**: fix-runtime (for test failures)
- **File includes**: Test coverage analysis, failing test details, testing strategy

### fix-security
- **Reads**: build-code handoffs
- **Creates**: Security vulnerability assessments and compliance reports
- **Handoff to**: build-code (for fixes)
- **File includes**: Vulnerability assessment, remediation priorities, compliance gaps

### profile-performance
- **Creates**: Performance analysis and optimization recommendations
- **Handoff to**: build-code (for optimizations)
- **File includes**: Performance metrics, bottleneck analysis, optimization strategies

### learn-docs
- **Reads**: All agent handoffs for comprehensive project context
- **Creates**: Documentation summaries and maintenance guides
- **Handoff to**: Final documentation deliverables
- **File includes**: Documentation structure, key content areas, maintenance recommendations

### build-spreadsheet
- **Creates**: Spreadsheet analysis and data extraction results
- **Handoff to**: plan-requirements (data context), build-code (data integration), learn-docs (data documentation)
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