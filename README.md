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

### 🟣 Planning & Architecture

#### **plan-requirements** (Opus)
**When to use**: Transform vague user requests into comprehensive requirements and actionable development plans
- Extracts true problems from requested solutions
- Defines functional and non-functional requirements (performance, security, usability)
- Identifies edge cases, dependencies, and constraints
- Creates structured requirements documents with acceptance criteria
- Establishes clear project scope and success criteria
- **Creates**: `requirements.md` with problem statement, user flows, and assumptions
- **Hands off to**: api-designer, code-implementer

#### **plan-api** (Opus)
**When to use**: Designs logical, consistent APIs and specifications that serve as contracts between system components
- Creates comprehensive API specifications with method signatures, parameters, and return types
- Ensures consistency across interface methods and data structures
- Designs data models, validation schemas, and error handling patterns
- Documents API usage patterns, examples, and evolution strategy
- Validates API usability through common workflow analysis
- **Flexible input**: Can work from requirements, existing codebase analysis, or standalone specifications
- **Reads from**: requirements-analyst (when designing from requirements)
- **Hands off to**: code-implementer

### 🟢 Building & Implementation

#### **build-cli** (Sonnet)
**When to use**: Design command-line interfaces with excellent user experience
- Creates intuitive command structures and argument hierarchies
- Designs configuration systems with clear precedence rules
- Generates shell completions and help documentation
- Ensures consistent error messaging and user guidance
- Follows progressive disclosure principles for complex CLIs
- **Reads from**: plan-requirements, plan-api
- **Creates**: `.agent-handoffs/build-cli-<uuid>.md` with CLI specifications
- **Hands off to**: build-code, learn-docs

#### **build-gui** (Sonnet)
**When to use**: Design modern graphical user interfaces with cross-platform compatibility and accessibility
- Designs component hierarchies and state management patterns
- Selects appropriate frameworks and ensures cross-platform compatibility
- Defines accessibility standards and responsive design strategies
- Optimizes for performance and maintainability
- Creates comprehensive UI specifications and design systems
- **Reads from**: plan-requirements, plan-api
- **Creates**: `.agent-handoffs/build-gui-<uuid>.md` with GUI specifications
- **Hands off to**: build-code, check-tests, learn-docs

#### **build-code** (Sonnet)
**When to use**: Transform requirements into clean, maintainable, production-ready code
- Implements code following "Make it work → Make it right → Make it fast" principle
- Follows existing codebase conventions and patterns
- Implements proper error handling, validation, and resource management
- Creates self-documenting code with clear naming and structure
- Applies SOLID principles and avoids common anti-patterns
- **Reads from**: plan-requirements, plan-api handoffs
- **Hands off to**: check-quality, fix-build

### 🔵 Knowledge & Documentation

#### **learn-codebase** (Haiku)
**When to use**: Rapidly explore and map unfamiliar codebases to provide architectural understanding and structural overview
- Maps project directory structure and organization patterns using fast tooling (fd, rg, tree)
- Identifies key modules, packages, and their purposes with API surface discovery
- Recognizes architectural patterns (MVC, microservices, layered architecture)
- Maps dependencies, technology stack, and module relationships
- Discovers entry points, configuration files, and build systems
- **Exploration strategy**: Structure → Patterns → Dependencies → Entry Points → Technology Detection
- **Output**: Project overview, module inventory, architecture patterns, technology stack summary
- **Hands off to**: plan-requirements (system context), build-code (conventions), learn-docs (architecture)

#### **learn-docs** (Sonnet)
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

#### **build-spreadsheet** (Sonnet)
**When to use**: Analyze, summarize, extract data from, or identify issues in spreadsheet files (.xlsx, .xlsm, .csv, .ods)
- Parses and analyzes spreadsheet files using appropriate libraries (openpyxl, pandas, odfpy)
- Extracts, transforms, and summarizes data from complex multi-sheet workbooks
- Detects data quality issues: inconsistent types, formatting problems, formula errors, circular references
- Creates reusable Python tools for common spreadsheet operations
- Handles edge cases: corrupted files, massive datasets, encrypted content, non-standard formats
- **Analysis methodology**: Assess → Structure → Quality → Report
- **Output**: Data structure summary, quality issues found, extraction tools created
- **Hands off to**: plan-requirements (data context), build-code (data integration), learn-docs (data documentation)

#### **learn-commits** (Haiku)
**When to use**: Search git history for deleted, moved, or modified code artifacts
- Efficiently searches for deleted or modified functions, variables, classes using `git log -S` and `-G`
- Tracks code evolution and changes over time with historical context
- Locates moved or renamed code artifacts using `git log --follow`
- Provides complete code snippets with commit references and dates
- **Search strategy**: Interprets intent, chooses appropriate git method, extracts results systematically
- **Output**: Search summary, historical findings with evolution notes, and commit references
- **Hands off to**: Any agent needing historical context or code recovery

### 🟡 Quality Assurance

#### **check-quality** (Sonnet)
**When to use**: Review code for maintainability issues, anti-patterns, and quality concerns
- Identifies code smells and anti-patterns (god objects, long methods, deep nesting, duplicated code)
- Assesses maintainability, readability, and structural quality
- Evaluates testability, error handling, and proper abstractions
- Provides prioritized refactoring recommendations (critical, major, minor issues)
- Focuses on genuine maintainability impact over stylistic preferences
- **Reads from**: build-code handoffs
- **Output**: Quality assessment with specific file:line references and actionable feedback
- **Hands off to**: build-code (for improvements)

#### **check-tests** (Haiku)
**When to use**: Creates and executes comprehensive test suites for thorough code validation
- Generates unit, integration, and end-to-end tests following Arrange-Act-Assert pattern
- Supports multiple frameworks (Jest, Vitest, pytest, Go testing, Rust built-in)
- Executes test suites and analyzes results with coverage reporting
- Identifies failing tests, coverage gaps, and performance bottlenecks
- Targets 80%+ line coverage with 100% coverage for critical business logic
- **Testing methodology**: Analyze → Categorize → Design → Implement → Execute → Report
- **Reads from**: build-code handoffs
- **Output**: Test summary, failed test details, coverage gaps, and performance metrics
- **Hands off to**: fix-runtime (for test failures)

### 🟠 Operations & Monitoring

#### **profile-performance** (Sonnet)
**When to use**: Identifies performance bottlenecks and optimization opportunities across applications and systems
- Profiles CPU usage, memory consumption, I/O patterns, and concurrency issues
- Analyzes runtime performance patterns, execution flows, and resource utilization
- Identifies slow queries, operations, algorithmic inefficiencies, and memory leaks
- Uses appropriate profiling tools (Chrome DevTools, py-spy, go tool pprof, perf, etc.)
- **Analysis process**: Baseline → Profile → Analyze → Prioritize → Recommend
- **Optimization categories**: Algorithmic, Database, Memory, I/O, Caching, Concurrency
- **Metrics focus**: P50/P95/P99 latencies, throughput, resource usage, error rates
- **Output**: Performance summary, critical bottlenecks, optimization opportunities, implementation priorities
- **Hands off to**: build-code (for optimizations)

### 🔴 Incident Response

#### **fix-runtime** (Opus)
**When to use**: Investigate runtime bugs, crashes, and production issues with systematic analysis
- Analyzes runtime exceptions, stack traces, and crash dumps
- Investigates performance issues, memory leaks, and resource problems
- Debugs race conditions, timing bugs, and concurrency issues
- Traces user-reported behavioral issues through code execution paths
- **NOT for**: Build/compilation errors (use fix-build)
- **Output**: Clear problem statement, root cause analysis, and specific fix recommendations
- **Hands off to**: build-code (for fixes)

#### **fix-build** (Haiku)
**When to use**: Build projects and analyze compilation/build errors with automatic system detection
- Detects build systems automatically (npm/yarn, Maven, Gradle, Make, Cargo, CMake)
- Executes appropriate build commands and captures output
- Classifies errors by category (syntax, type, reference, dependency, configuration, linking)
- Provides specific, actionable fix recommendations for each error
- **NOT for**: Runtime issues (use fix-runtime)
- **Output**: Build status, critical errors, error classification, and fix recommendations
- **Hands off to**: build-code (for fixes)

#### **fix-security** (Sonnet)
**When to use**: Scans for security vulnerabilities and compliance issues with defensive security focus
- Identifies OWASP Top 10 and common security vulnerabilities across web apps, APIs, and databases
- Reviews authentication, authorization, session management, and access control mechanisms
- Detects exposed secrets, credentials, and sensitive data using static analysis tools
- Validates input sanitization, output encoding, and injection prevention measures
- Assesses cryptographic implementations, SSL/TLS configurations, and secure communication
- **Assessment areas**: Authentication/Authorization, Data Protection, Infrastructure Security, Code Security
- **Priority levels**: Critical → High → Medium → Low with specific remediation guidance
- **Reads from**: build-code handoffs
- **Output**: Executive summary, critical vulnerabilities, detailed findings, and compliance status
- **Hands off to**: build-code (for security fixes)

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

### Method 1: Git Clone with Symlinks
Clone this repository and create symlinks to your agents directory for easy updates:

```bash
# Clone the repository
git clone https://github.com/LeonardMH/claude-code-agents.git ~/claude-code-agents

# Create symlinks to agents directory
mkdir -p ~/.claude/agents && ln -sf ~/claude-code-agents/*.md ~/.claude/agents/
```

To update agents: `cd ~/claude-code-agents && git pull`

### Method 2: Manual Download
Download individual agent `.md` files from this repository and place them in your Claude Code agents directory (`~/.claude/agents/`). They'll be available for use the next time you start `claude`.

### Directory Structure
After installation, your `~/.claude/agents/` directory will contain all agent `.md` files and supporting documentation.

## Workflow Examples

### New Feature Development
1. **plan-requirements** (Opus) → Transform vague request into clear requirements with acceptance criteria
2. **plan-api** (Opus) → Design consistent interfaces and data models (if needed)
3. **build-code** (Sonnet) → Build clean, maintainable implementation following best practices
4. **fix-build** (Haiku) → Ensure code compiles and builds successfully
5. **check-tests** (Haiku) → Create comprehensive test suite with good coverage
6. **check-quality** (Sonnet) → Quality assessment and refactoring recommendations
7. **fix-security** (Sonnet) → Security vulnerability assessment (for security-sensitive features)
8. **learn-docs** (Sonnet) → Create user-facing documentation and API docs

### CLI Tool Development
1. **plan-requirements** (Opus) → Define CLI tool requirements and user workflows
2. **build-cli** (Sonnet) → Design command structure, arguments, and user interaction patterns
3. **build-code** (Sonnet) → Build the CLI application following design specifications
4. **check-tests** (Haiku) → Create CLI integration and command tests
5. **learn-docs** (Sonnet) → Generate user guides and man pages

### GUI Application Development
1. **plan-requirements** (Opus) → Define UI requirements and user stories
2. **build-gui** (Sonnet) → Design component architecture, state management, and accessibility patterns
3. **build-code** (Sonnet) → Build UI components and application logic
4. **check-tests** (Haiku) → Create GUI tests and validate accessibility
5. **learn-docs** (Sonnet) → Create user guides and component documentation

### Bug Investigation & Fix
1. **fix-runtime** (Opus) → Systematic analysis of runtime issue with root cause identification
2. **learn-commits** (Haiku) → Find historical context or previous implementations (if needed)
3. **build-code** (Sonnet) → Implement targeted fix based on root cause analysis
4. **check-tests** (Haiku) → Add regression tests to prevent future occurrences
5. **check-quality** (Sonnet) → Review fix for quality and unintended side effects

### Performance Optimization
1. **profile-performance** (Sonnet) → Identify bottlenecks using appropriate profiling tools
2. **build-code** (Sonnet) → Implement optimizations based on profiling results
3. **check-tests** (Haiku) → Validate performance improvements and ensure functionality
4. **check-quality** (Sonnet) → Review optimized code for maintainability

### Legacy Code Analysis
1. **learn-codebase** (Haiku) → Map current codebase structure and identify key architectural patterns
2. **learn-commits** (Haiku) → Search for historical implementations and evolution patterns
3. **check-quality** (Sonnet) → Assess current code quality and identify improvement opportunities
4. **fix-security** (Sonnet) → Check for security vulnerabilities in legacy code
5. **learn-docs** (Sonnet) → Document findings and create improvement roadmap

### Unfamiliar Codebase Integration
1. **learn-codebase** (Haiku) → Rapidly map project structure, modules, and architectural patterns
2. **plan-requirements** (Opus) → Analyze how new requirements fit within existing architecture
3. **plan-api** (Opus) → Design interfaces that align with existing patterns (if needed)
4. **build-code** (Sonnet) → Implement following discovered conventions and patterns
5. **check-tests** (Haiku) → Create tests using existing testing frameworks and patterns

### Data Analysis & Integration
1. **build-spreadsheet** (Sonnet) → Analyze data structure, quality issues, and extract insights from spreadsheet files
2. **plan-requirements** (Opus) → Transform data insights into feature requirements
3. **plan-api** (Opus) → Design data models and interfaces for spreadsheet integration
4. **build-code** (Sonnet) → Build data processing and integration features
5. **check-tests** (Haiku) → Validate data processing accuracy and edge cases
