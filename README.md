# Claude Code Agents

A specialized collection of focused agents for software development workflows.
Each agent handles a specific aspect of development with clear boundaries and
inter-agent communication capabilities.

## Agent Architecture

Agents are organized by color according to their role in the software development lifecycle and use different Claude models based on their complexity requirements:

### Model Selection Rationale
- **Opus**: Complex reasoning tasks requiring deep analysis and strategic thinking (planning, debugging, architectural decisions)
- **Sonnet**: Balanced tasks requiring both capability and efficiency (building, reviewing, documentation, security analysis)
- **Haiku**: Fast, focused tasks with clear objectives (searching, testing, exploring, analyzing)

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
- **Hands off to**: plan-api, build-code

#### **plan-api** (Opus)
**When to use**: Designs logical, consistent APIs and specifications that serve as contracts between system components
- Creates comprehensive API specifications with method signatures, parameters, and return types
- Ensures consistency across interface methods and data structures
- Designs data models, validation schemas, and error handling patterns
- Documents API usage patterns, examples, and evolution strategy
- Validates API usability through common workflow analysis
- **Flexible input**: Can work from requirements, existing codebase analysis, or standalone specifications
- **Reads from**: plan-requirements (when designing from requirements)
- **Hands off to**: build-code

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
- **Hands off to**: build-code, write-docs

#### **build-gui** (Sonnet)
**When to use**: Design modern graphical user interfaces with cross-platform compatibility and accessibility
- Designs component hierarchies and state management patterns
- Selects appropriate frameworks and ensures cross-platform compatibility
- Defines accessibility standards and responsive design strategies
- Optimizes for performance and maintainability
- Creates comprehensive UI specifications and design systems
- **Reads from**: plan-requirements, plan-api
- **Creates**: `.agent-handoffs/build-gui-<uuid>.md` with GUI specifications
- **Hands off to**: build-code, check-tests, write-docs

#### **build-code** (Sonnet)
**When to use**: Transform requirements into clean, maintainable, production-ready code
- Implements code following "Make it work → Make it right → Make it fast" principle
- Follows existing codebase conventions and patterns
- Implements proper error handling, validation, and resource management
- Creates self-documenting code with clear naming and structure
- Applies SOLID principles and avoids common anti-patterns
- **Reads from**: plan-requirements, plan-api handoffs
- **Hands off to**: check-quality, fix-build

#### **build-hardware** (Sonnet)
**When to use**: Design hardware interfaces and communication protocols for embedded systems and IoT devices
- Designs hardware communication protocols and device drivers
- Creates hardware abstraction layers for cross-platform compatibility
- Interfaces with sensors, actuators, and embedded systems
- Handles real-time data acquisition and control systems
- Ensures reliable hardware-software integration patterns
- **Reads from**: plan-requirements, plan-api handoffs
- **Creates**: `.agent-handoffs/build-hardware-<uuid>.md` with hardware specifications
- **Hands off to**: build-code, check-tests, write-docs

#### **build-slides** (Sonnet)
**When to use**: Design and create professional presentations with compelling visual storytelling and accessibility
- Creates slide hierarchies and information architecture for presentations
- Implements effective data visualizations and infographics
- Designs presentation templates and visual design systems
- Ensures accessibility compliance and cross-platform compatibility
- Optimizes presentations for different delivery contexts (in-person, virtual, self-guided)
- **Reads from**: plan-requirements, learn-slides handoffs
- **Creates**: `.agent-handoffs/build-slides-<uuid>.md` with presentation specifications
- **Hands off to**: build-code (for interactive elements), write-docs, check-ui

#### **build-documents** (Sonnet)
**When to use**: Create professional documents including Word (.docx), OpenDocument (.odt), and PDF files with sophisticated formatting
- Designs document templates and style systems for consistent branding
- Creates complex documents with advanced formatting, tables, and interactive features
- Implements forms, automation pipelines, and document generation workflows
- Builds template libraries and cross-format compatibility systems
- Ensures accessibility compliance and professional quality output
- **Reads from**: plan-requirements, learn-documents handoffs
- **Creates**: `.agent-handoffs/build-documents-<uuid>.md` with document specifications
- **Hands off to**: build-code (for automation), check-tests, write-docs

#### **build-spreadsheet** (Sonnet)
**When to use**: Create powerful, maintainable spreadsheets with robust formulas, data models, and automation
- Designs logical spreadsheet structures and data models
- Implements complex formulas, calculations, and data processing logic
- Creates automated workflows with macros and scripting
- Builds reusable templates and standardized formats
- Ensures data integrity, validation, and error handling
- **Reads from**: plan-requirements, learn-spreadsheet handoffs
- **Creates**: `.agent-handoffs/build-spreadsheet-<uuid>.md` with spreadsheet specifications
- **Hands off to**: build-code (for integrations), check-tests, write-docs

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
- **Hands off to**: plan-requirements (system context), build-code (conventions), write-docs (architecture)

#### **write-docs** (Sonnet)
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


#### **learn-commits** (Haiku)
**When to use**: Search git history for deleted, moved, or modified code artifacts
- Efficiently searches for deleted or modified functions, variables, classes using `git log -S` and `-G`
- Tracks code evolution and changes over time with historical context
- Locates moved or renamed code artifacts using `git log --follow`
- Provides complete code snippets with commit references and dates
- **Search strategy**: Interprets intent, chooses appropriate git method, extracts results systematically
- **Output**: Search summary, historical findings with evolution notes, and commit references
- **Hands off to**: Any agent needing historical context or code recovery

#### **learn-documents** (Haiku)
**When to use**: Analyze, extract content from, or understand document files (.docx, .doc, .odt, .pdf)
- Parses and analyzes documents using appropriate libraries (python-docx, odfpy, PyPDF2/pdfplumber)
- Extracts text, images, tables, forms, and metadata from documents
- Identifies document structure, formatting patterns, and organizational hierarchy
- Processes interactive forms, fillable PDFs, and complex layouts with OCR support
- Detects document quality issues, accessibility problems, and formatting inconsistencies
- **Analysis methodology**: Assess → Structure → Content → Format → Report
- **Output**: Document structure summary, content extraction, formatting patterns, accessibility assessment
- **Hands off to**: plan-requirements (document context), build-documents (template creation), write-docs (content documentation)

#### **learn-slides** (Haiku)
**When to use**: Analyze, understand, or extract information from presentation files (.pptx, .odp, .key, .pdf presentations)
- Parses and analyzes presentations using appropriate libraries (python-pptx, odfpy, PyPDF2)
- Extracts content, images, charts, and multimedia from slides
- Identifies design patterns, color schemes, branding elements, and template usage
- Analyzes slide structure, hierarchies, and content organization
- Categorizes slide types and assesses design consistency
- **Analysis methodology**: Assess → Structure → Content → Design → Report
- **Output**: Presentation structure summary, content extraction, design patterns identified
- **Hands off to**: plan-requirements (presentation context), build-slides (template creation), write-docs (presentation documentation)

#### **learn-spreadsheet** (Sonnet)
**When to use**: Analyze, summarize, extract data from, or identify issues in spreadsheet files (.xlsx, .xlsm, .csv, .ods)
- Parses and analyzes spreadsheet files using appropriate libraries (openpyxl, pandas, odfpy)
- Extracts, transforms, and summarizes data from complex multi-sheet workbooks
- Detects data quality issues: inconsistent types, formatting problems, formula errors, circular references
- Creates reusable Python tools for common spreadsheet operations
- Handles edge cases: corrupted files, massive datasets, encrypted content, non-standard formats
- **Analysis methodology**: Assess → Structure → Quality → Report
- **Output**: Data structure summary, quality issues found, extraction tools created
- **Hands off to**: plan-requirements (data context), build-code (data integration), write-docs (data documentation)

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

#### **check-ui** (Haiku)
**When to use**: Automated GUI testing with visual regression, accessibility validation, and user experience testing
- Executes visual regression testing and screenshot comparison
- Validates accessibility compliance and keyboard navigation
- Tests user interaction flows and UI responsiveness
- Verifies cross-browser and cross-platform compatibility
- Measures UI performance and animation smoothness
- **Reads from**: build-gui, build-code handoffs
- **Creates**: `.agent-handoffs/check-ui-<uuid>.md` with UI test results
- **Hands off to**: build-code (for fixes), write-docs (for test documentation)

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
1. **plan-requirements** → Transform vague request into clear requirements with acceptance criteria
2. **plan-api** → Design consistent interfaces and data models (if needed)
3. **build-code** → Build clean, maintainable implementation following best practices
4. **fix-build** → Ensure code compiles and builds successfully
5. **check-tests** → Create comprehensive test suite with good coverage
6. **check-quality** → Quality assessment and refactoring recommendations
7. **fix-security** → Security vulnerability assessment (for security-sensitive features)
8. **write-docs** → Create user-facing documentation and API docs

### CLI Tool Development
1. **plan-requirements** → Define CLI tool requirements and user workflows
2. **build-cli** → Design command structure, arguments, and user interaction patterns
3. **build-code** → Build the CLI application following design specifications
4. **check-tests** → Create CLI integration and command tests
5. **write-docs** → Generate user guides and man pages

### GUI Application Development
1. **plan-requirements** → Define UI requirements and user stories
2. **build-gui** → Design component architecture, state management, and accessibility patterns
3. **build-code** → Build UI components and application logic
4. **check-ui** → Execute visual regression testing and accessibility validation
5. **check-tests** → Create comprehensive test coverage for functionality
6. **write-docs** → Create user guides and component documentation

### Hardware Integration Development
1. **plan-requirements** → Define hardware interface requirements and constraints
2. **build-hardware** → Design communication protocols and hardware abstraction layers
3. **build-code** → Implement device drivers and hardware interfaces
4. **check-tests** → Create hardware simulation tests and integration validation
5. **write-docs** → Document hardware setup and troubleshooting procedures

### Bug Investigation & Fix
1. **fix-runtime** → Systematic analysis of runtime issue with root cause identification
2. **learn-commits** → Find historical context or previous implementations (if needed)
3. **build-code** → Implement targeted fix based on root cause analysis
4. **check-tests** → Add regression tests to prevent future occurrences
5. **check-quality** → Review fix for quality and unintended side effects

### Performance Optimization
1. **profile-performance** → Identify bottlenecks using appropriate profiling tools
2. **build-code** → Implement optimizations based on profiling results
3. **check-tests** → Validate performance improvements and ensure functionality
4. **check-quality** → Review optimized code for maintainability

### Legacy Code Analysis
1. **learn-codebase** → Map current codebase structure and identify key architectural patterns
2. **learn-commits** → Search for historical implementations and evolution patterns
3. **check-quality** → Assess current code quality and identify improvement opportunities
4. **fix-security** → Check for security vulnerabilities in legacy code
5. **write-docs** → Document findings and create improvement roadmap

### Unfamiliar Codebase Integration
1. **learn-codebase** → Rapidly map project structure, modules, and architectural patterns
2. **plan-requirements** → Analyze how new requirements fit within existing architecture
3. **plan-api** → Design interfaces that align with existing patterns (if needed)
4. **build-code** → Implement following discovered conventions and patterns
5. **check-tests** → Create tests using existing testing frameworks and patterns

### Data Analysis & Integration
1. **learn-spreadsheet** → Analyze data structure, quality issues, and extract insights from spreadsheet files
2. **plan-requirements** → Transform data insights into feature requirements
3. **plan-api** → Design data models and interfaces for spreadsheet integration
4. **build-code** → Build data processing and integration features
5. **check-tests** → Validate data processing accuracy and edge cases

### Presentation Creation & Automation
1. **plan-requirements** → Define presentation objectives, audience, and content requirements
2. **learn-slides** → Analyze existing templates, brand guidelines, or reference presentations
3. **build-slides** → Design presentation structure, templates, and visual hierarchy
4. **build-code** → Generate dynamic content, data visualizations, or interactive elements
5. **check-ui** → Validate accessibility and cross-platform compatibility
6. **write-docs** → Document presentation templates and usage guidelines

### Spreadsheet Development & Automation
1. **plan-requirements** → Define data processing needs, calculations, and business rules
2. **learn-spreadsheet** → Analyze existing data sources and current spreadsheet processes
3. **build-spreadsheet** → Create robust spreadsheet models with formulas and validation
4. **build-code** → Implement automation scripts, macros, or external integrations
5. **check-tests** → Validate calculations, edge cases, and data integrity
6. **write-docs** → Create user guides and maintenance documentation

### Business Reporting & Analytics Workflow
1. **learn-spreadsheet** → Analyze current data sources and reporting requirements
2. **plan-requirements** → Define comprehensive reporting and analytics requirements
3. **build-spreadsheet** → Create data processing and calculation engines
4. **build-slides** → Design executive dashboards and presentation templates
5. **build-code** → Implement automated data pipelines and report generation
6. **check-tests** → Validate data accuracy and report consistency
7. **write-docs** → Document reporting processes and maintenance procedures

### Document Processing & Automation
1. **learn-documents** → Analyze existing forms, contracts, or document templates
2. **plan-requirements** → Define document automation and generation requirements
3. **build-documents** → Create document templates, forms, and generation systems
4. **build-code** → Implement document automation pipelines and data integration
5. **check-tests** → Validate document generation, forms, and data processing
6. **write-docs** → Document template usage and maintenance procedures

### Complete Office Suite Integration
1. **learn-documents** → Extract data and requirements from existing Word/PDF documents
2. **learn-spreadsheet** → Analyze data processing needs from extracted information
3. **plan-requirements** → Define comprehensive office automation workflow
4. **build-spreadsheet** → Create data processing and calculation models
5. **build-documents** → Generate professional reports and documentation
6. **build-slides** → Create presentation templates from document content
7. **build-code** → Implement end-to-end office automation system
8. **check-tests** → Validate entire workflow from data input to document output
9. **write-docs** → Document complete office automation system
