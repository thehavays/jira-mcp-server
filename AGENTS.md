# Jira MCP Server — Agent & Developer Guide

This document is the primary onboarding and operational guide for AI agents and developers working in the `jira-mcp-server` repository.

---

## 📖 Project Overview

Jira MCP Server is a Model Context Protocol (MCP) server for Atlassian Jira issue tracking and development status. It connects AI assistants to Jira instances to search issues, retrieve details, update statuses/fields, manage comments, fetch development status commits (from Bitbucket, Crucible, GitHub, GitLab), and download attachments.

### 🛠️ Core Technology Stack
- **Language / Runtime**: Node.js (ES Modules, Node 20+)
- **Protocol**: Model Context Protocol (MCP) via `@modelcontextprotocol/sdk`
- **HTTP Client**: `axios`
- **Testing**: `vitest`

---

## 📁 Repository Structure

```
jira-mcp-server/
├── .agents/                 # Agent rules, workflows, and standards
│   ├── GIT_WORKFLOW.md      # Detailed Git branching & GitHub MCP rules
│   ├── CI.md                # Continuous Integration standards & quality gates
│   └── CD.md                # Continuous Deployment & hosting architecture
├── .github/
│   ├── workflows/           # Automated CI/CD actions (ci.yml)
│   ├── ISSUE_TEMPLATE/      # GitHub issue forms (bug_report.yml, feature_request.yml)
│   └── pull_request_template.md
├── scripts/                 # Development and test helper scripts
├── tests/                   # Vitest unit & security test suites
├── .editorconfig            # Standard editor formatting rules
├── .env.example             # Documented environment configuration
├── .gitignore               # Ignored dependencies, caches, and secrets
├── .jira-config.json        # Security permissions config (projects & tools filter)
├── index.js                 # MCP server entrypoint and tool implementations
├── LICENSE                  # MIT License
├── package.json             # Dependencies and project scripts
└── README.md                # User & contributor documentation
```

---

## ⚡ Canonical Commands

Always use the following commands for development and verification:

| Action | Command | Purpose |
| :--- | :--- | :--- |
| **Start Server** | `npm start` | Starts MCP server on stdio transport |
| **Run Tests** | `npm run test` | Executes unit and security test suites with Vitest |
| **Watch Tests** | `npm run test:watch` | Runs test runner in watch mode |
| **Syntax Check** | `node -c index.js` | Validates JavaScript syntax and module integrity |

---

## 🌿 Git & Version Control Workflow

For complete version control guidelines, see [.agents/GIT_WORKFLOW.md](./.agents/GIT_WORKFLOW.md).

### Summary Rules:
1. **Protected Branches**: Direct pushes to `main` and `dev` are strictly forbidden.
2. **Branch Roles**:
   - `dev`: Primary integration branch. All feature and fix branches must originate from and target `dev`.
   - `main`: Production release branch. Updated only by merging `dev` via Release Pull Request.
3. **Branch Naming**: `<type>/<kebab-case-description>` (e.g. `feat/add-tool`, `fix/jql-query`).
4. **Commit Format**: Conventional Commits (`feat: ...`, `fix: ...`, `chore: ...`).
5. **Pre-flight Verification**: Always run `npm run test` before opening PRs.
6. **User Confirmation Gates (MANDATORY)**:
   - **Before Opening PR**: ALWAYS ask user approval before calling `create_pull_request`.
   - **Before Merging PR**: ALWAYS ask user approval before calling `merge_pull_request`.
7. **Issue Closure Policy**: Issues are created first with `issue_write`, referenced in dev PRs (`ref #...`), and closed ONLY when merged to `main` via Release PR (`closes #...`).

---

## 🔄 CI/CD Guidelines
- **CI**: See [.agents/CI.md](./.agents/CI.md) for quality gates.
- **CD**: See [.agents/CD.md](./.agents/CD.md) for distribution mappings.
