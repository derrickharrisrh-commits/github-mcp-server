# Architecture Overview

```mermaid
flowchart LR
    User["User or AI Client"] --> Client["MCP Client / Host"]
    Client --> Server["GitHub MCP Server"]

    subgraph ServerLayer["Server Layer"]
        Transport["MCP Transport\nstdio / session handling"]
        Registry["Tool Registry\nGitHub capabilities"]
        Router["Request Router\nvalidation & dispatch"]
    end

    Server --> Transport
    Transport --> Registry
    Registry --> Router

    subgraph GitHubIntegration["GitHub Integration Layer"]
        Auth["Authentication\nPAT / token context"]
        API["GitHub API Clients\nREST / GraphQL"]
        Tools["Repository, Issue, PR,\nWorkflow, and Metadata Tools"]
    end

    Router --> Auth
    Auth --> Tools
    Tools --> API

    subgraph GitHubPlatform["GitHub Platform"]
        Repo["Repositories"]
        Issues["Issues & Pull Requests"]
        Actions["Actions / Workflows"]
        Meta["Users, Teams, and Metadata"]
    end

    API --> Repo
    API --> Issues
    API --> Actions
    API --> Meta

    Repo --> Result["Structured Tool Response"]
    Issues --> Result
    Actions --> Result
    Meta --> Result
    Result --> Transport
    Transport --> Client
    Client --> User
```

## Overview

This architecture follows a simple request-response flow:

- An AI client or user sends a request through the MCP interface.
- The GitHub MCP server receives the request and routes it through the tool registry.
- Authenticated GitHub API calls are executed to access repositories, issues, pull requests, workflows, and related metadata.
- Results are transformed into structured responses and returned to the client.

The design keeps the protocol layer separate from the GitHub integration layer, making it easier to extend capabilities without changing the overall client experience.
