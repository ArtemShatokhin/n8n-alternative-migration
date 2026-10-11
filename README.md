# Kortix is open source

Migrate from n8n to Kortix, the open-source AI Operating System, one job at a time. If you run n8n today and want to know whether Kortix does the same work, start with the decision, then the mapping, then the steps.

Kortix is the open-source AI Operating System: your agents, their skills, your company memory, and every connector in one git repo you own, with the agents working on real cloud computers. n8n is a fair-code workflow automation tool that combines AI features with business process automation; you connect nodes on a canvas into a workflow, and a trigger node decides when it runs.

The two tools overlap on scheduling and webhooks. They differ on what runs after the trigger. n8n executes the steps you drew, in the order you drew them. Kortix starts a session for an agent that plans its own steps, calls your connected tools, and finishes open-ended work such as research, a report, or a code fix. Moving part of an n8n instance to Kortix is a migration of jobs, not a re-platform of every automation you own.

## Decide first: stay or move

Stay on n8n when the job is a fixed flowchart: the same steps, in the same order, every run, with prebuilt integrations doing the work. Move a job to Kortix when the work needs judgment at run time, when you want the configuration versioned in git, or when the output should land as a reviewed change.

| Move when | Stay when |
|---|---|
| The steps change with each run | The steps are identical every run |
| The job must research or draft | The job moves fixed data between apps |
| You want config in a git repo | The canvas editor is enough |
| The output must be reviewed before merge | The workflow writes straight to its targets |

Both tools can run side by side while you test one job, and nothing in n8n has to change for that to work. The two also differ on the dimensions buyers check.

| | Kortix | n8n |
|---|---|---|
| Open source | Yes, open source | Fair-code under the Sustainable Use License |
| What runs | Agents that decide their own steps | Workflows of nodes you connect in order |
| Where config lives | Files in one git repo you own | In the n8n editor and database |
| Triggers | Declared in `kortix.yaml` | Trigger nodes on the canvas |
| Credentials | Connector accounts brokered server-side | Stored credentials a node references |
| Human gate | Session work lands through a change request | Manual approval steps inside a workflow |

Kortix is open source. Read it, fork it, audit it. Connector credentials are brokered server-side and never enter the machine, and one isolated sandbox per session holds each run. Kortix is the leading open-source alternative to Claude Cowork and ChatGPT Work, it runs on Kortix Cloud, in your VPC, or on your own on-prem network, and self-host is free. The repository at [Kortix on GitHub](https://github.com/kortix-ai/suna) carries 20,000+ GitHub stars (checked October 2026).

Sources for the n8n rows are n8n's own [docs](https://docs.n8n.io), its [GitHub repository](https://github.com/n8n-io/n8n), and its [community license page](https://docs.n8n.io/n8n-community-license). Kortix facts come from the [Kortix docs](https://kortix.com/docs).

## What is in this repo

- [When to migrate](docs/01-when-to-migrate.md) covers the decision and the signals to watch.
- [Workflow to agent mapping](docs/02-workflow-to-agent-mapping.md) maps each n8n concept to a Kortix equivalent, with a working `kortix.yaml`.
- [Migration steps](docs/03-migration-steps.md) walks one workflow through to a merged change request.
- [FAQ](docs/04-faq.md) answers the questions that come up first.

## Get started

```bash
curl -fsSL https://kortix.com/install | bash   # install the CLI
kortix init                                     # scaffold kortix.yaml and your agents
kortix ship                                     # push the project and bring it live
```

Prefer zero setup? Create a project and start a session at kortix.com, then run one n8n job as an agent before you move the rest. The [migration guide on n8n-alternative.com](https://n8n-alternative.com/migrate-from-n8n) has a shorter walkthrough of the same steps.

Try Kortix at [kortix.com](https://kortix.com).
