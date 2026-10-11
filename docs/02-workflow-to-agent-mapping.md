# Mapping n8n concepts to Kortix

n8n is a fair-code workflow automation tool: its own docs define an n8n workflow as "a collection of nodes connected together to automate a process," and the building blocks of that workflow are nodes and the connections between them ([n8n docs](https://docs.n8n.io/build/understand-workflows)). Kortix is the open-source AI Operating System: a project is a git repo with a `kortix.yaml` manifest at the root, and the agents, skills, memory and connectors live in that repo. Migrating a job from n8n to Kortix means translating the pieces of a workflow into the pieces of an agent. The table on this page names each equivalent, and the manifest further on runs a scheduled agent.

## The mapping

| n8n concept | What it is in n8n | Kortix equivalent |
|---|---|---|
| Workflow | A collection of nodes connected to automate a process | An agent: `agents/<name>.md` plus its `kortix.yaml` block |
| Trigger | A node that decides when the workflow runs | A `triggers` entry of type `cron` or `webhook` |
| Node | A building block inside the workflow, wired to the next | A connector the agent calls as a tool |
| Credential | Stored authentication details the workflow needs | A connector account; the credential brokered server-side |
| Execution | One run of a workflow, saved for viewing and debugging | A session: one isolated sandbox on its own branch |

Each n8n row follows n8n's own description of workflow components, credentials and executions ([n8n docs](https://docs.n8n.io/build/understand-workflows)). The two products differ again on where the configuration lives, and the [n8n vs Kortix breakdown](https://n8n-alternative.com/n8n-vs-kortix) takes that apart row by row.

A workflow in n8n answers "what is the next step?" from a graph a person drew. A Kortix agent answers the same question at run time from a goal. That difference is why the node-to-connector mapping is not one to one: several n8n nodes that fetch, transform and post data become one connector call the agent makes when it decides the call is needed. The judgment that used to sit in the length of the canvas moves into the agent's `.md` file.

## A working schedule trigger

An agent is `agents/<name>.md` (behavior) plus an `agents.<name>` block in `kortix.yaml` (governance). A trigger has exactly two types: `cron` and `webhook`. This manifest runs a digest agent every weekday morning:

```yaml
# yaml-language-server: $schema=https://kortix.com/schema/kortix.v2.schema.json
kortix_version: 2
default_agent: digest

project:
  name: daily-digest
  description: A scheduled agent that summarizes the day and opens a change request.

agents:
  digest:
    file: agents/digest.md   # behavior: the system prompt and the model
    connectors: all          # governance: what the agent may reach
    secrets: all

triggers:
  - slug: daily-digest
    type: cron
    agent: digest
    cron: '0 0 9 * * 1-5'
    timezone: America/Los_Angeles
    prompt: |
      Summarize yesterday's support tickets and open a change request with the digest.
```

`default_agent` must name a declared agent, and the `agents` block holds governance only: which agents may launch and what each may touch. Every behavioral field, such as the model and the system prompt, lives in the agent's `.md` file, never in `kortix.yaml`, and the validator rejects a governance block that carries one ([manifest reference](https://kortix.com/docs/project/manifest)).

A webhook trigger is the same block with `type: webhook`, so an n8n webhook node maps to it. A trigger's `session_mode` has four values: `fresh`, `reuse`, `pinned`, `keyed`. The default is `fresh`. A trigger has no field that posts the result to a channel, so the agent puts its output where the work belongs: a change request, a reply, or a file in the repo.

Connector credentials are brokered server-side and never enter the machine, so an agent reaches a system through a connector account without holding its key. Kortix reaches 3,000+ apps in a click, plus MCP, OpenAPI, Postman, GraphQL and raw HTTP ([connectors](https://kortix.com/docs/connect/connectors)).

Related reading: [migrating from n8n to Kortix](https://n8n-alternative.com/migrate-from-n8n).
