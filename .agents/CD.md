# Continuous Deployment (CD) Guidelines for Jira MCP Server

---

## ⚙️ Environment & CD Context

* **HOSTING_PLATFORM**: Local CLI / MCP Client Stdio Execution
* **GITHUB_USERNAME**: `thehavays`
* **REPOSITORY**: `jira-mcp-server`
* **DEPLOYMENT_TYPE**: Package distribution (npm / npx) & local repository execution
* **RUNTIME**: Node.js 20+

---

## 🌐 Delivery & Runtime Architecture

`jira-mcp-server` operates as a Model Context Protocol (MCP) server communicating over Standard Input/Output (`stdio`). 
It connects MCP hosts (such as Claude Desktop, Cursor, Gemini Code Assist, or Antigravity) to Atlassian Jira instances.

### Distribution Methods:
1. **Direct GitHub Execution via `npx`**:
   ```json
   {
     "mcpServers": {
       "jira": {
         "command": "npx",
         "args": ["-y", "github:thehavays/jira-mcp-server"],
         "env": {
           "JIRA_URL": "https://jira.example.com/",
           "JIRA_USERNAME": "your_username",
           "JIRA_PASSWORD": "your_secure_password_or_token",
           "JIRA_AUTH_TYPE": "bearer"
         }
       }
     }
   }
   ```
2. **Local Repository Execution**:
   ```json
   {
     "mcpServers": {
       "jira": {
         "command": "node",
         "args": ["/path/to/jira-mcp-server/index.js"],
         "env": {
           "JIRA_URL": "https://jira.example.com/",
           "JIRA_USERNAME": "your_username",
           "JIRA_PASSWORD": "your_secure_password_or_token",
           "JIRA_AUTH_TYPE": "bearer"
         }
       }
     }
   }
   ```

---

## 🌿 Environment & Branch Mappings

| Branch | Stage | Purpose |
| :--- | :--- | :--- |
| **`main`** | **Production Release** | Stable release code; consumers installing via `github:thehavays/jira-mcp-server` pull from `main`. |
| **`dev`** | **Integration / Staging** | Active development and integration branch for verified feature PRs. |
| **`feat/*`** | **Feature Branches** | Work in progress branches targeting `dev`. |

---

## 🚀 Production Release Workflow

1. **Verify Integration**: Ensure all features and tests pass on `dev`.
2. **Open Release PR**: Ask user confirmation, then create Release PR from `dev` to `main` via GitHub MCP `create_pull_request`.
3. **Merge Release PR**: Ask user confirmation, then merge via GitHub MCP `merge_pull_request` (auto-closes resolved issues).
4. **Tag Release**: Create semantic version tag (e.g., `git tag -a v1.1.0 -m "Release v1.1.0"`).
