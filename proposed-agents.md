# Proposed Additional Agents for Complete Development Workflow

Based on the existing agents, here are strategic additions to fill workflow gaps:

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
5. **test-specialist** → create and run test suite
6. **security-auditor** → security review
7. **performance-profiler** → optimize performance
8. **code-reviewer** → final quality check
9. **deployment-orchestrator** → deploy to production

### Legacy Code Modernization
1. **code-reviewer** → assess current state and recommend improvements
2. **git-archaeologist** → understand historical context
3. **security-auditor** → identify vulnerabilities
4. **performance-profiler** → find bottlenecks
5. **code-implementer** → modernize code based on recommendations
6. **test-specialist** → add comprehensive test coverage
7. **build-error-analyzer** → ensure builds work
8. **deployment-orchestrator** → safe deployment

### Production Issue Response
1. **runtime-debugger** → analyze the issue
2. **performance-profiler** → check for performance degradation
3. **security-auditor** → rule out security issues
4. **git-archaeologist** → check recent changes
5. **code-implementer** → implement hotfix
6. **test-specialist** → validate fix
7. **deployment-orchestrator** → emergency deployment
