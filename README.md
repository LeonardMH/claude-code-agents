# Claude Code Agents

A specialized collection of focused agents for software development workflows.
Each agent handles a specific aspect of development with clear boundaries and
inter-agent communication capabilities.

## Available Agents

### **requirements-analyst**
**When to use**: Transform vague user requests into clear, testable requirements
- Extracts true problems from requested solutions
- Defines functional and non-functional requirements
- Identifies edge cases and dependencies
- Creates structured requirements documents
- **Hands off to**: code-implementer

### **code-implementer**
**When to use**: Transform requirements into production-ready code
- Implements code from requirements documents
- Follows existing codebase conventions
- Handles error cases and validation
- Creates clean, maintainable code
- **Reads from**: requirements-analyst
- **Hands off to**: code-reviewer, build-error-analyzer

### **runtime-debugger**
**When to use**: Investigate runtime bugs, crashes, and production issues
- Analyzes runtime exceptions and stack traces
- Investigates performance issues and memory leaks
- Debugs race conditions and timing bugs
- **NOT for**: Build/compilation errors (use build-error-analyzer)
- **Hands off to**: code-implementer (for fixes)

### **build-error-analyzer**
**When to use**: Build projects and analyze compilation/build errors
- Detects build systems automatically
- Classifies and analyzes build errors
- Provides specific fix recommendations
- **NOT for**: Runtime issues (use runtime-debugger)
- **Hands off to**: code-implementer (for fixes)

### **code-reviewer**
**When to use**: Review code for quality, maintainability, and anti-patterns
- Identifies code smells and anti-patterns
- Assesses maintainability and readability
- Provides refactoring recommendations
- Evaluates testability and error handling
- **Reads from**: code-implementer
- **Hands off to**: code-implementer (for improvements)

### **git-archaeologist**
**When to use**: Search git history for deleted, moved, or modified code
- Finds previous implementations and configurations
- Tracks code evolution over time
- Locates moved or renamed code artifacts
- Provides historical context for decisions
- **Hands off to**: Any agent needing historical context

### **test-specialist**
**When to use**: Creates and executes comprehensive test suites
- Generates unit, integration, and edge case tests
- Runs test suites and analyzes results
- Identifies failing tests and coverage gaps
- Manages test environments and frameworks
- **Reads from**: code-implementer
- **Hands off to**: runtime-debugger (for test failures)

## Agent Communication

All agents communicate through the `.agent-handoffs/` shared workspace using structured markdown files. This enables:
- Context preservation between agents
- Findings and recommendations sharing
- Coordinated multi-agent workflows

See `handoff-system.md` for detailed documentation.

## Installation

Drop the `.md` files into your Claude Code agents directory and they'll be
available for use the next time you start `claude`.

## Workflow Examples

### New Feature Development
1. **requirements-analyst** → clarify requirements
2. **code-implementer** → build the feature
3. **build-error-analyzer** → ensure it builds
4. **test-specialist** → create and run tests
5. **code-reviewer** → quality check

### Bug Investigation
1. **runtime-debugger** → analyze the issue
2. **git-archaeologist** → check historical context (if needed)
3. **code-implementer** → implement fix
4. **code-reviewer** → review the fix
