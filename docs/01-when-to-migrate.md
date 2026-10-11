# When to migrate from n8n to Kortix

n8n is a fair-code workflow automation tool that runs a workflow, which its docs define as a collection of nodes connected together to automate a process. Kortix is the open-source AI Operating System: agents, skills, company memory, and connectors in one git repo you own, with each session on its own cloud computer. A move from n8n to Kortix is mostly a change of job type. n8n runs the steps you drew, and Kortix runs an agent that decides the steps. The two overlap on scheduling and webhooks and diverge on everything the trigger starts.

## The distinction that decides it

A workflow engine answers "what is the next step?" from a graph you built. An agent answers "what is the next step?" at run time from a goal you wrote. For a fixed sequence, the graph is the strength: predictable, inspectable, cheap to re-run. When the sequence should change with each input, the graph becomes work you maintain, because every new case adds a branch and an expression.

Kortix replaces the graph with an agent. The agent plans, calls your connectors as tools, and runs multi-step work to a finish. The deterministic parts stay as text: which agents exist, which connectors each may reach, and what fires them, all declared in `kortix.yaml`. The judgment parts live in the agent's markdown file and the skills it follows.

## Concrete signals to move a job

Move one job at a time, and only when you can point at a signal.

1. The same workflow runs, but a person opens the editor after most failures to change the steps. The job needs a decision, and you are making it by hand.
2. You keep adding If and Code nodes to make a fixed graph behave like judgment. Each addition is a case the engine cannot learn from.
3. You want the automation versioned, diffable, and reviewable in git, rather than clicked into an editor and a database.
4. The job needs a real machine: install a dependency, reproduce a failure, run tests, open a change against a repository.
5. You need a human gate on what changes, at the level of a diff a person reads before it lands.
6. Credentials should be brokered server-side, so a tool call runs without the sandbox holding the key.

## Concrete signals to stay on n8n

Stay where the workflow is already the right shape.

1. The steps are the same every run, in the same order, and prebuilt n8n integrations do the work. [n8n's repository](https://github.com/n8n-io/n8n) advertises 1,500+ integrations and 9,000+ workflow templates.
2. You rely on the visual canvas as the source of truth and do not want the configuration in a repo.
3. The automation moves fixed data between services and never needs a model to decide anything.
4. Compliance or habit keeps the run history in n8n, and nobody needs the output reviewed as code.

Neither choice is permanent. n8n and Kortix can run side by side: leave the fixed flows in n8n, and start one Kortix agent on a cron schedule or a webhook for the open-ended job. A migration moves jobs one at a time, with no cutover date.

n8n is fair-code under the Sustainable Use License, which allows free use, modification, and redistribution with limits on commercial hosting and on products whose value comes from n8n itself; [n8n's community license page](https://docs.n8n.io/n8n-community-license) states the terms. Kortix is open source, so you can read it, fork it, and audit it. If the side-by-side comparison is what you came for, the [n8n vs Kortix breakdown](https://n8n-alternative.com/n8n-vs-kortix) covers the two point by point, and the [workflow to agent mapping](02-workflow-to-agent-mapping.md) names each equivalent.
