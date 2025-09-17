# Claude Code Agents

A specialized collection of focused agents for software development workflows.
Each agent handles a specific aspect of development with clear boundaries and
inter-agent communication capabilities.

## Color Scheme

Agents are organized by color according to their role in the software development lifecycle:

| Color | Role | Agents |
|-------|------|---------|
| 🟣 Purple | Planning & Architecture | requirements-analyst, api-designer |
| 🟢 Green | Building & Implementation | code-implementer |
| 🔵 Blue | Knowledge & Documentation | documentation-writer, git-archaeologist |
| 🟡 Yellow | Quality Assurance | code-reviewer, test-specialist |
| 🟠 Orange | Operations & Monitoring | performance-profiler |
| 🔴 Red | Incident Response | runtime-debugger, build-error-analyzer, security-auditor |

## Available Agents

### **requirements-analyst**
**When to use**: Transform vague user requests into clear, testable requirements
- Extracts true problems from requested solutions
- Defines functional and non-functional requirements
- Identifies edge cases and dependencies
- Creates structured requirements documents
- **Hands off to**: api-designer, code-implementer

### **api-designer**
**When to use**: Designs logical, consistent APIs and specifications
- Creates API specifications and interface contracts
- Ensures API consistency and best practices
- Designs data models and validation schemas
- Documents API usage patterns and evolution strategy
- **Reads from**: requirements-analyst (when designing from requirements)
- **Hands off to**: code-implementer

### **code-implementer**
**When to use**: Transform requirements into production-ready code
- Implements code from requirements documents
- Follows existing codebase conventions
- Handles error cases and validation
- Creates clean, maintainable code
- **Reads from**: requirements-analyst, api-designer
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

### **security-auditor**
**When to use**: Scans for security vulnerabilities and compliance issues
- Identifies OWASP Top 10 and common security vulnerabilities
- Reviews authentication and authorization mechanisms
- Detects exposed secrets, credentials, and sensitive data
- Validates input sanitization and output encoding
- **Reads from**: code-implementer
- **Hands off to**: code-implementer (for fixes)

### **performance-profiler**
**When to use**: Identifies performance bottlenecks and optimization opportunities
- Profiles CPU usage, memory consumption, and I/O patterns
- Analyzes runtime performance patterns and execution flows
- Identifies slow queries, operations, and algorithmic inefficiencies
- Suggests targeted optimization strategies
- **Hands off to**: code-implementer (for optimizations)

### **documentation-writer**
**When to use**: Creates comprehensive project documentation and user-facing guides
- Generates user-facing documentation and README files
- Creates API documentation from code specifications
- Writes inline code documentation and tutorials
- Updates setup guides and architectural documentation
- **Reads from**: All agents for comprehensive documentation
- **Hands off to**: Final documentation deliverables

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
2. **api-designer** → design interfaces (if needed)
3. **code-implementer** → build the feature
4. **build-error-analyzer** → ensure it builds
5. **test-specialist** → create and run tests
6. **code-reviewer** → quality check
7. **documentation-writer** → document the feature

### Bug Investigation
1. **runtime-debugger** → analyze the issue
2. **git-archaeologist** → check historical context (if needed)
3. **code-implementer** → implement fix
4. **code-reviewer** → review the fix
