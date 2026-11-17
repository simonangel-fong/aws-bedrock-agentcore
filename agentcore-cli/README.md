# AgentCore: `agentcore cli`

[Back](../README.md)

- [AgentCore: `agentcore cli`](#agentcore-agentcore-cli)
  - [Create project](#create-project)
    - [Understand the created files](#understand-the-created-files)
  - [Run the agent locally](#run-the-agent-locally)
  - [Build \& Deploy](#build--deploy)
  - [Invoke deployed agent](#invoke-deployed-agent)
  - [Operation - View logs and traces](#operation---view-logs-and-traces)
  - [Clean up](#clean-up)

---

## Create project

- ref: https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/agentcore-get-started-cli.html
  - Project name: TripPlanner
  - template: Python Strands
  - git: skip git as using a global .git instead of .git per lab

```sh
cd agentcore-cli

# create a Code-based agent project: using python strands template
agentcore create --name TripPlanner --template agent-python-strands --skip-git
```

---

### Understand the created files

```
MyProject/
├── .gitignore
├── agentcore/
│   ├── .env.local          # API keys (gitignored)
│   ├── agentcore.json      # Project specification (agents, memories, etc.)
│   ├── aws-targets.json    # Deployment targets (region, account)
│   └── cdk/                # CDK infrastructure (auto-managed)
└── app/
    └── agent/
        ├── .gitignore
        ├── README.md
        ├── main.py           # Agent entry point
        ├── memory/
        │   ├── __init__.py
        │   └── session.py    # AgentCore Memory session manager and retrieval settings.
        ├── model/
        │   ├── __init__.py
        │   └── load.py       # Model configuration (Claude on Bedrock)
        ├── parse.py
        └── pyproject.toml    # Python dependencies
```

---

## Run the agent locally

- CLI automatically:
  - Creates a Python virtual environment (.venv) if needed
  - Installs dependencies via uv sync
  - Start the local development server.

```sh
cd TripPlanner

# test with browser
agentcore dev --mode browser

# invoke the local agent.
agentcore invoke --runtime agent --local --payload "{\"prompt\":\"Plan two days in Lisbon in one paragraph.\"}"
```

> Debug: Authentication failed: Please make sure your API Key is valid.
> issue: could be the left-over API key
> Solution: remove the API key
> ps command:
> `[Environment]::SetEnvironmentVariable('AWS_BEARER_TOKEN_BEDROCK', $null, 'User')`
> `Remove-Item Env:AWS_BEARER_TOKEN_BEDROCK`

![dev_browser](./img/dev_browser.png)

---

## Build & Deploy

- Build deployable artifacts

```sh
agentcore build
```

- create files in `agentcore/cdk/cdk.out/`, ignoring by `.gitignore`

---

- Deploy
  1. Packages your code into a zip artifact
  2. Uses `AWS CDK` under the hood to synthesize and provision resources
  3. Creates an `AgentCore Runtime` endpoint for your agent
  4. Configures `CloudWatch` logging and observability

```sh
agentcore deploy

# verify
agentcore status --json
```

- Confirm in console

![console01](./img/console01.png)

---

## Invoke deployed agent

- in console

![console02](./img/console02.png)

```sh
agentcore invoke --runtime agent --payload "{\"prompt\":\"Hello, what can you do?\"}"
# data: {"event": {"messageStart": {"role": "assistant"}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": "Hi"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": "!"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": " I can help with a"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": " range"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": " of things"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": ":"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": "\n\n- **Answering"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": " questions**"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": " on"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": " general"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": " topics, such"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": " as science, history, t"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": "ech and"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": " writing.\n- **Writ"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": "ing and editing**"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": ", including emails, summ"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": "aries, stories and"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": " essays.\n- **Br"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": "ainstorming and"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": " planning**, like ideas"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": ", out"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": "lines and"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": " decision-making.\n- **"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": "Expl"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": "aining concepts** in"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": " pl"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": "ain language.\n- **Co"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": "ding help"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": "**, including writing,"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": " debugging and expl"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": "aining code.\n- **"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": "Math"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": "**"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": ", including adding two num"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": "bers with"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": " a built"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": "-in add"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": "ition tool.\n\nIf you'"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": "d like, give"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": " me two"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": " numbers and I'"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": "ll add"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": " them,"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": " or t"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": "ell me what"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": " you're"}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockDelta": {"delta": {"text": " working on."}, "contentBlockIndex": 0}}}
# data: {"event": {"contentBlockStop": {"contentBlockIndex": 0}}}
# data: {"event": {"messageStop": {"stopReason": "end_turn"}}}
# data: {"event": {"metadata": {"usage": {"inputTokens": 434, "outputTokens": 186, "totalTokens": 620}, "metrics": {"latencyMs": 2041}}}}
# status=200 content-type=text/event-stream; charset=utf-8 runtime-session-id=098c0979-5dda-4cc7-b79c-b2369a806748 mcp-session-id=- mcp-protocol-version=- trace-id=- trace-parent=- trace-state=- baggage=- complete=true bytes=5070
```

---

## Operation - View logs and traces

```sh
# Stream Runtime logs
agentcore logs runtime

# Search a past window
agentcore logs runtime --since 30m --level error
agentcore logs runtime --since 1h --query "timeout"

# List recent Runtime traces
agentcore traces runtime list --since 30m
# TRACE ID                          TIMESTAMP             SESSION ID
# 6afb99173153031c52dfcc0543282f36  2026-11-17 00:00:32Z  098c0979-5dda-4cc7-b79c-b2369a806748
# 6afb98d53de49b5b49e6990e43666188  2026-11-16 23:59:29Z  8bbd0b7b-4297-4452-9eb8-7bfb50a06d01
# 6afb97f8606371de392b3e4c3542bd79  2026-11-16 23:55:40Z  a6ce53b6-6d9e-4c8d-854c-0080063b9d0c
# 6afb974467f02e5c65c4d86f123bb533  2026-11-16 23:52:46Z  9922d89b-7004-46cf-a3c1-d1a9709dbae6
# 6afb96d276172c227104f44722a3192c  2026-11-16 23:50:48Z  475a8b25-5380-4a0f-b0a4-b6324bddfcd4

# Download a specific trace
agentcore traces runtime get 6afb99173153031c52dfcc0543282f36
```

---

## Clean up

```sh
# removes every resource from the project specification.
agentcore remove all --yes
agentcore deploy --yes
# ✓ synthesizing CloudFormation templates
# ✓ removing stack AgentCore-MyProject-default
# removed project 'MyProject' from target 'default'
```
