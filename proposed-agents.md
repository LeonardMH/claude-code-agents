# Proposed Additional Agents for Complete Development Workflow

Based on the existing agents, here are strategic additions to fill workflow gaps:

## **test-generator**
**When to use**: Creates comprehensive test suites from implemented code
- Generates unit, integration, and edge case tests
- Follows existing test framework conventions
- Analyzes code coverage requirements
- **Reads from**: code-implementer
- **Hands off to**: test-runner

## **test-runner**
**When to use**: Executes test suites and analyzes results
- Runs test suites and reports results
- Identifies failing tests and coverage gaps
- Provides detailed failure diagnostics
- Manages test environments
- **Reads from**: test-generator, code-implementer
- **Hands off to**: runtime-debugger (for test failures)

## **dependency-manager**
**When to use**: Analyzes and updates project dependencies
- Updates project dependencies safely
- Identifies security vulnerabilities
- Manages version conflicts and compatibility
- Suggests dependency upgrades and alternatives
- **Hands off to**: build-error-analyzer (after updates)

## **performance-profiler**
**When to use**: Identifies performance bottlenecks and optimization opportunities
- Profiles CPU and memory usage
- Analyzes runtime performance patterns
- Identifies slow queries and operations
- Suggests optimization strategies
- **Hands off to**: code-implementer (for optimizations)

## **security-auditor**
**When to use**: Scans for security vulnerabilities and compliance issues
- Identifies OWASP top 10 vulnerabilities
- Reviews authentication and authorization
- Checks for exposed secrets and credentials
- Validates input sanitization
- **Hands off to**: code-implementer (for fixes)

## **api-designer**
**When to use**: Designs RESTful/GraphQL APIs and specifications
- Creates API specifications (OpenAPI/GraphQL)
- Ensures API consistency and best practices
- Designs endpoint structures
- Documents API contracts
- **Hands off to**: code-implementer

## **database-architect**
**When to use**: Designs database schemas and optimizes queries
- Designs normalized database schemas
- Creates and manages migrations
- Optimizes queries and indexes
- Analyzes database performance
- **Hands off to**: code-implementer

## **documentation-writer**
**When to use**: Creates comprehensive project documentation
- Generates user-facing documentation
- Creates API documentation from code
- Writes inline code documentation
- Updates README and setup guides
- **Reads from**: all agents

## **refactoring-specialist**
**When to use**: Performs large-scale code refactoring and modernization
- Extracts reusable components and utilities
- Implements design patterns
- Modernizes legacy code
- Reduces technical debt
- **Reads from**: code-reviewer
- **Hands off to**: build-error-analyzer

## **deployment-orchestrator**
**When to use**: Manages deployment pipelines and infrastructure
- Configures CI/CD pipelines
- Handles containerization (Docker/Kubernetes)
- Manages deployment environments
- Orchestrates rollbacks and scaling
- **Reads from**: build-error-analyzer, test-runner

## Complete Workflow Examples

### Full Feature Development
1. **requirements-analyst** → clarify requirements
2. **api-designer** → design API contracts
3. **database-architect** → design data layer
4. **code-implementer** → implement feature
5. **test-generator** → create test suite
6. **test-runner** → validate implementation
7. **security-auditor** → security review
8. **performance-profiler** → optimize performance
9. **code-reviewer** → final quality check
10. **documentation-writer** → document feature
11. **deployment-orchestrator** → deploy to production

### Legacy Code Modernization
1. **code-reviewer** → assess current state
2. **git-archaeologist** → understand history
3. **security-auditor** → identify vulnerabilities
4. **performance-profiler** → find bottlenecks
5. **refactoring-specialist** → modernize code
6. **dependency-manager** → update dependencies
7. **test-generator** → add missing tests
8. **build-error-analyzer** → ensure builds work
9. **deployment-orchestrator** → safe deployment

### Production Issue Response
1. **runtime-debugger** → analyze the issue
2. **performance-profiler** → check for performance degradation
3. **security-auditor** → rule out security issues
4. **git-archaeologist** → check recent changes
5. **code-implementer** → implement hotfix
6. **test-runner** → validate fix
7. **deployment-orchestrator** → emergency deployment