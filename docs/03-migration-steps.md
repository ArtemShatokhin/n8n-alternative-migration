# Migrating a workflow from n8n to Kortix, step by step

n8n is a fair-code workflow automation tool that runs a workflow, which n8n's docs define as a collection of nodes connected together to automate a process, with a trigger node deciding when it runs ([n8n docs](https://docs.n8n.io/build/understand-workflows)). Kortix is the open-source AI Operating System, and a project is a git repo with a `kortix.yaml` manifest at the root, where an agent is `agents/<name>.md` (behavior) plus a governance block in the manifest. Migration moves jobs one at a time. Nothing in your n8n instance has to change, and there is no cutover date.

The five steps on this page take one n8n workflow to a merged change request. The [migration guide at n8n-alternative.com](https://n8n-alternative.com/) has the reader-facing version of the same path.

## 1. Inventory the workflows

List the workflows in your n8n instance and, for each one, write down the trigger node and the nodes it calls. n8n's docs cover how to save, publish and version a workflow and how to view and debug its executions ([n8n docs](https://docs.n8n.io/build/understand-workflows)), so an execution is your record of what the job actually does today. Group the workflows by trigger type, because a cron schedule and a webhook migrate through the same two steps.

Then pick one workflow to move first. The strongest candidate is the one a person has to fix by hand after most failures: the fixed flows are comfortable in n8n, and the open-ended job is the one that gains an agent.

## 2. Map the triggers

In n8n a trigger node starts the workflow. In Kortix a trigger declares the same thing in `kortix.yaml`, and a trigger has exactly two types: `cron` and `webhook`. A scheduled n8n workflow becomes a `type: cron` trigger with the same cron expression, and a webhook-driven workflow becomes `type: webhook`. A trigger's `session_mode` has four values: `fresh`, `reuse`, `pinned`, `keyed`. The default is `fresh`. The [workflow to agent mapping](02-workflow-to-agent-mapping.md) names every equivalent, and the [manifest reference](https://kortix.com/docs/project/manifest) carries the full trigger schema.

## 3. Wire the connectors

An n8n node references a stored credential. A Kortix agent reaches the same system through a connector account, and connector credentials are brokered server-side and never enter the machine, so the agent gets a token and never the key. Kortix reaches 3,000+ apps in a click, plus MCP, OpenAPI, Postman, GraphQL and raw HTTP.

Grant the agent only what it needs. The `connectors` key in its `kortix.yaml` block is deny by default, so `connectors: all` grants full reach and a list of slugs grants a subset. Add the account from the project's Connectors screen, then move each n8n node to the connector call that does the same job ([connectors](https://kortix.com/docs/connect/connectors)).

## 4. Run one agent on a branch

Install the CLI and start a session:

```bash
curl -fsSL https://kortix.com/install | bash   # install the CLI
kortix init                                     # scaffold kortix.yaml and agents/
kortix sessions new --prompt "Reproduce the digest job and open a change request"
```

One isolated sandbox per session. Each session has its own isolated machine and branch, so the agent can install, run and break anything, and only what it commits survives. This is the step that replaces the canvas: instead of drawing the nodes, you write the agent's goal and let it choose the calls.

## 5. Land it through a change request

Session work reaches `main` through a change request, and merge is default-deny for agents. Review what the agent produced:

```bash
kortix cr ls                  # list what the session proposes
kortix cr diff <cr>           # read the unified patch
kortix cr merge <cr>          # land it on the default branch
```

A merged change request is the job running in production, versioned in git like the rest of the repo. When the first job behaves, repeat the loop for the next workflow. The two tools can run side by side the whole time, so the n8n workflow keeps running until you decide to switch it off. The [n8n alternatives comparison](https://n8n-alternative.com/best-n8n-alternatives) puts this path next to the other options.

Try Kortix at [kortix.com](https://kortix.com).
