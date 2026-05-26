# 🤖 ServiceNow Agent — Powered by Claude + GitHub Actions

A Claude-powered agent that runs in GitHub Actions. You type a natural language command, Claude figures out the intent, and calls the ServiceNow API automatically.

## What it can do

| You say | Agent does |
|---|---|
| `create a task for deployment review` | Creates a new SCTASK in ServiceNow |
| `create a high priority task for prod outage fix` | Creates task with High priority |
| `update SCTASK0010001 to in progress` | Updates that ticket's state |
| `close SCTASK0010001` | Closes the ticket |
| `get details of SCTASK0010001` | Fetches and shows ticket info |

---

## Setup

### 1. Add GitHub Secrets

Go to your repo → **Settings → Secrets and variables → Actions → New repository secret**

Add these 4 secrets:

| Secret Name | Value |
|---|---|
| `ANTHROPIC_API_KEY` | Your Anthropic API key from console.anthropic.com |
| `SNOW_INSTANCE` | Your PDI URL e.g. `dev12345.service-now.com` |
| `SNOW_USERNAME` | `admin` |
| `SNOW_PASSWORD` | Your PDI admin password |

### 2. Push this repo to GitHub

```bash
git init
git add .
git commit -m "feat: add servicenow agent"
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git
git push -u origin main
```

### 3. Run the Agent

1. Go to your repo on GitHub
2. Click **Actions** tab
3. Click **ServiceNow Agent** workflow
4. Click **Run workflow**
5. Type your command in the input box e.g.:
   - `create a task for reviewing the mm-soul-agent deployment`
   - `close SCTASK0010005 task is done`
6. Click **Run workflow**
7. Watch the logs — you'll see the ticket number and link!

---

## Project Structure

```
servicenow-agent/
├── .github/
│   └── workflows/
│       └── servicenow-agent.yml   # GitHub Actions workflow
└── agent/
    ├── index.js                   # Claude agent + agentic loop
    ├── servicenow.js              # ServiceNow REST API client
    └── package.json
```

## How it works

```
You type command in GitHub Actions
        ↓
Claude reads command + decides which tool to call
(create_task / update_task / close_task / get_task)
        ↓
Agent calls ServiceNow REST API
        ↓
Claude summarizes result with ticket number + link
        ↓
You see it in GitHub Actions logs
```

## ServiceNow Tables Used
- `sc_task` — Service Catalog Tasks

## Priority Mapping
- `1` = Critical
- `2` = High  
- `3` = Moderate (default)
- `4` = Low

## State Mapping
- `1` = Open
- `2` = Work in Progress
- `3` = Closed Complete
- `4` = Closed Incomplete
