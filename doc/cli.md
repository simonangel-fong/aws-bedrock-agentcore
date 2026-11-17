# AgentCore: agentcore cli

[Back](../README.md)

- [AgentCore: agentcore cli](#agentcore-agentcore-cli)
  - [Cheapsheets](#cheapsheets)

---

## Cheapsheets

| Command                              | Desc                     |
| ------------------------------------ | ------------------------ |
| `agentcore --region <region>`        | AWS region               |
| `agentcore --debug`                  | debug logging            |
| `agentcore --json`                   | JSON output              |
| `agentcore -h`,`agentcore --help`    | display help for command |
| `agentcore -V`,`agentcore --version` | display the CLI version  |

- **Project:**

| Command             | Desc                                                                                              |
| ------------------- | ------------------------------------------------------------------------------------------------- |
| `agentcore create`  | create a new project                                                                              |
| `agentcore add`     | add project resources                                                                             |
| `agentcore dev`     | develop and test project agents                                                                   |
| `agentcore deploy`  | build and deploy project resources to an AWS target                                               |
| `agentcore invoke`  | invoke a Runtime, harness, or Gateway                                                             |
| `agentcore status`  | inspect deployed project resources for an AWS target                                              |
| `agentcore logs`    | stream or search logs for resources in the current project                                        |
| `agentcore traces`  | inspect traces for resources in the current project                                               |
| `agentcore export`  | convert project resources into editable code you own                                              |
| `agentcore migrate` | migrate an AgentCore CLI v0 project to the current project format, keeping its deployed resources |
| `agentcore remove`  | remove a resource from the project                                                                |
| `agentcore build`   | build the project's deployable artifacts                                                          |

- **Resources:**

| Command              | Desc                                                       |
| -------------------- | ---------------------------------------------------------- |
| `agentcore harness`  | manage config-defined agents and their versions            |
| `agentcore runtime`  | inspect hosted agent code, its endpoints, and versions     |
| `agentcore gateway`  | inspect MCP tool gateways and their targets                |
| `agentcore identity` | manage API key and OAuth credential providers              |
| `agentcore memory`   | inspect agent memory: sessions, events, and records        |
| `agentcore eval`     | evaluate and optimize agents with evaluators and A/B tests |
| `agentcore payment`  | inspect payment managers, sessions, and instruments        |

- **Settings:**

| Command              | Desc                                      |
| -------------------- | ----------------------------------------- |
| `agentcore feedback` | send feedback about AgentCore to the team |
| `agentcore config`   | read/write global config values           |
| `agentcore update`   | check for and install CLI updates         |
