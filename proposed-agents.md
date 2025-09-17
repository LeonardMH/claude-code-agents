# Proposed Additional Agents for Complete Development Workflow

Based on the existing agents, here are strategic additions to fill workflow gaps:

## **performance-profiler**
**When to use**: Identifies performance bottlenecks and optimization opportunities
- Profiles CPU and memory usage
- Analyzes runtime performance patterns
- Identifies slow queries and operations
- Suggests optimization strategies
- **Hands off to**: code-implementer (for optimizations)

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

