# Proposed Additional Agents for Complete Development Workflow

Based on the existing agents, here are strategic additions to fill workflow gaps:

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

