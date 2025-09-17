# Claude Code Agents

A specialized collection of focused agents for software development workflows.
Each agent handles a specific aspect of development with clear boundaries and
inter-agent communication capabilities.

## Agent Architecture

Agents are organized by color according to their role in the software development lifecycle and use different Claude models based on their complexity requirements:

| Color | Role | Model |
|-------|------|-------|
| 🟣 Purple | Planning & Architecture | Opus |
| 🟢 Green | Building & Implementation | Sonnet |
| 🔵 Blue | Knowledge & Documentation | Sonnet, Haiku |
| 🟡 Yellow | Quality Assurance | Sonnet, Haiku |
| 🟠 Orange | Operations & Monitoring | Sonnet |
| 🔴 Red | Incident Response | Opus, Haiku, Sonnet |

## Available Agents

### **requirements-analyst** (Opus)
**When to use**: Transform vague user requests into comprehensive requirements and actionable development plans
- Extracts true problems from requested solutions
- Defines functional and non-functional requirements (performance, security, usability)
- Identifies edge cases, dependencies, and constraints
- Creates structured requirements documents with acceptance criteria
- Establishes clear project scope and success criteria
- **Creates**: `requirements.md` with problem statement, user flows, and assumptions
- **Hands off to**: api-designer, code-implementer

### **api-designer** (Opus)
**When to use**: Designs logical, consistent APIs and specifications that serve as contracts between system components
- Creates comprehensive API specifications with method signatures, parameters, and return types
- Ensures consistency across interface methods and data structures
- Designs data models, validation schemas, and error handling patterns
- Documents API usage patterns, examples, and evolution strategy
- Validates API usability through common workflow analysis
- **Flexible input**: Can work from requirements, existing codebase analysis, or standalone specifications
- **Reads from**: requirements-analyst (when designing from requirements)
- **Hands off to**: code-implementer

### **code-implementer** (Sonnet)
**When to use**: Transform requirements into clean, maintainable, production-ready code
- Implements code following "Make it work → Make it right → Make it fast" principle
- Follows existing codebase conventions and patterns
- Implements proper error handling, validation, and resource management
- Creates self-documenting code with clear naming and structure
- Applies SOLID principles and avoids common anti-patterns
- **Reads from**: requirements-analyst, api-designer handoffs
- **Hands off to**: code-reviewer, build-error-analyzer, test-specialist

### **runtime-debugger** (Opus)
**When to use**: Investigate runtime bugs, crashes, and production issues with systematic analysis
- Analyzes runtime exceptions, stack traces, and crash dumps
- Investigates performance issues, memory leaks, and resource problems
- Debugs race conditions, timing bugs, and concurrency issues
- Traces user-reported behavioral issues through code execution paths
- **NOT for**: Build/compilation errors (use build-error-analyzer)
- **Output**: Clear problem statement, root cause analysis, and specific fix recommendations
- **Hands off to**: code-implementer (for fixes)

### **build-error-analyzer** (Haiku)
**When to use**: Build projects and analyze compilation/build errors with automatic system detection
- Detects build systems automatically (npm/yarn, Maven, Gradle, Make, Cargo, CMake)
- Executes appropriate build commands and captures output
- Classifies errors by category (syntax, type, reference, dependency, configuration, linking)
- Provides specific, actionable fix recommendations for each error
- **NOT for**: Runtime issues (use runtime-debugger)
- **Output**: Build status, critical errors, error classification, and fix recommendations
- **Hands off to**: code-implementer (for fixes)

### **code-reviewer** (Sonnet)
**When to use**: Review code for maintainability issues, anti-patterns, and quality concerns
- Identifies code smells and anti-patterns (god objects, long methods, deep nesting, duplicated code)
- Assesses maintainability, readability, and structural quality
- Evaluates testability, error handling, and proper abstractions
- Provides prioritized refactoring recommendations (critical, major, minor issues)
- Focuses on genuine maintainability impact over stylistic preferences
- **Reads from**: code-implementer handoffs
- **Output**: Quality assessment with specific file:line references and actionable feedback
- **Hands off to**: code-implementer (for improvements)

### **git-archaeologist** (Haiku)
**When to use**: Search git history for deleted, moved, or modified code artifacts
- Efficiently searches for deleted or modified functions, variables, classes using `git log -S` and `-G`
- Tracks code evolution and changes over time with historical context
- Locates moved or renamed code artifacts using `git log --follow`
- Provides complete code snippets with commit references and dates
- **Search strategy**: Interprets intent, chooses appropriate git method, extracts results systematically
- **Output**: Search summary, historical findings with evolution notes, and commit references
- **Hands off to**: Any agent needing historical context or code recovery

### **test-specialist** (Haiku)
**When to use**: Creates and executes comprehensive test suites for thorough code validation
- Generates unit, integration, and end-to-end tests following Arrange-Act-Assert pattern
- Supports multiple frameworks (Jest, Vitest, pytest, Go testing, Rust built-in)
- Executes test suites and analyzes results with coverage reporting
- Identifies failing tests, coverage gaps, and performance bottlenecks
- Targets 80%+ line coverage with 100% coverage for critical business logic
- **Testing methodology**: Analyze → Categorize → Design → Implement → Execute → Report
- **Reads from**: code-implementer handoffs
- **Output**: Test summary, failed test details, coverage gaps, and performance metrics
- **Hands off to**: runtime-debugger (for test failures)

### **security-auditor** (Sonnet)
**When to use**: Scans for security vulnerabilities and compliance issues with defensive security focus
- Identifies OWASP Top 10 and common security vulnerabilities across web apps, APIs, and databases
- Reviews authentication, authorization, session management, and access control mechanisms
- Detects exposed secrets, credentials, and sensitive data using static analysis tools
- Validates input sanitization, output encoding, and injection prevention measures
- Assesses cryptographic implementations, SSL/TLS configurations, and secure communication
- **Assessment areas**: Authentication/Authorization, Data Protection, Infrastructure Security, Code Security
- **Priority levels**: Critical → High → Medium → Low with specific remediation guidance
- **Reads from**: code-implementer handoffs
- **Output**: Executive summary, critical vulnerabilities, detailed findings, and compliance status
- **Hands off to**: code-implementer (for security fixes)

### **performance-profiler** (Sonnet)
**When to use**: Identifies performance bottlenecks and optimization opportunities across applications and systems
- Profiles CPU usage, memory consumption, I/O patterns, and concurrency issues
- Analyzes runtime performance patterns, execution flows, and resource utilization
- Identifies slow queries, operations, algorithmic inefficiencies, and memory leaks
- Uses appropriate profiling tools (Chrome DevTools, py-spy, go tool pprof, perf, etc.)
- **Analysis process**: Baseline → Profile → Analyze → Prioritize → Recommend
- **Optimization categories**: Algorithmic, Database, Memory, I/O, Caching, Concurrency
- **Metrics focus**: P50/P95/P99 latencies, throughput, resource usage, error rates
- **Output**: Performance summary, critical bottlenecks, optimization opportunities, implementation priorities
- **Hands off to**: code-implementer (for optimizations)

### **documentation-writer** (Sonnet)
**When to use**: Creates comprehensive project documentation and user-facing guides that make code accessible
- Generates user-facing documentation, README files, and setup guides
- Creates API documentation from code specifications with practical examples
- Writes inline code documentation, tutorials, and architectural documentation
- Maintains consistency across all documentation using clear, simple language
- **Writing principles**: Clarity, Completeness, Consistency, Currency, Examples
- **Analysis process**: Assess → Understand → Plan → Write → Validate
- **Reads from**: All agent handoffs for comprehensive project context
- **Output**: Documentation summary, structure overview, key sections, maintenance notes
- **Hands off to**: Final documentation deliverables and maintenance recommendations

## Agent Communication

All agents communicate through the `.agent-handoffs/` shared workspace using structured markdown files with the naming convention `<agent-role-name>-<short-uuid>.md`. This system enables:

- **Context preservation**: Each agent documents findings, decisions, and recommendations
- **Coordinated workflows**: Agents read relevant handoffs before starting work
- **Knowledge sharing**: Historical analysis and implementation context passed between agents
- **Quality tracking**: Implementation details and testing needs communicated across the pipeline

### Handoff File Structure
Each handoff file contains:
- **Summary**: Brief overview of work completed
- **Key Findings**: Important discoveries or results
- **Context for Next Agent**: What the next agent needs to know
- **Recommendations**: Specific actions or considerations
- **Files/Components Affected**: Relevant code locations with line numbers

See `handoff-system.md` for complete documentation and agent-specific handoff patterns.

## Installation

Drop the `.md` files into your Claude Code agents directory and they'll be
available for use the next time you start `claude`.

## Workflow Examples

### New Feature Development
1. **requirements-analyst** (Opus) → Transform vague request into clear requirements with acceptance criteria
2. **api-designer** (Opus) → Design consistent interfaces and data models (if needed)
3. **code-implementer** (Sonnet) → Build clean, maintainable implementation following best practices
4. **build-error-analyzer** (Haiku) → Ensure code compiles and builds successfully
5. **test-specialist** (Haiku) → Create comprehensive test suite with good coverage
6. **code-reviewer** (Sonnet) → Quality assessment and refactoring recommendations
7. **security-auditor** (Sonnet) → Security vulnerability assessment (for security-sensitive features)
8. **documentation-writer** (Sonnet) → Create user-facing documentation and API docs

### Bug Investigation & Fix
1. **runtime-debugger** (Opus) → Systematic analysis of runtime issue with root cause identification
2. **git-archaeologist** (Haiku) → Find historical context or previous implementations (if needed)
3. **code-implementer** (Sonnet) → Implement targeted fix based on root cause analysis
4. **test-specialist** (Haiku) → Add regression tests to prevent future occurrences
5. **code-reviewer** (Sonnet) → Review fix for quality and unintended side effects

### Performance Optimization
1. **performance-profiler** (Sonnet) → Identify bottlenecks using appropriate profiling tools
2. **code-implementer** (Sonnet) → Implement optimizations based on profiling results
3. **test-specialist** (Haiku) → Validate performance improvements and ensure functionality
4. **code-reviewer** (Sonnet) → Review optimized code for maintainability

### Legacy Code Analysis
1. **git-archaeologist** (Haiku) → Search for historical implementations and evolution patterns
2. **code-reviewer** (Sonnet) → Assess current code quality and identify improvement opportunities
3. **security-auditor** (Sonnet) → Check for security vulnerabilities in legacy code
4. **documentation-writer** (Sonnet) → Document findings and create improvement roadmap
