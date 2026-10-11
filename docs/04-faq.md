# Migrating from n8n: FAQ

n8n is a fair-code workflow automation tool that runs a workflow, a collection of nodes connected together to automate a process, started when a trigger node fires ([n8n docs](https://docs.n8n.io/build/understand-workflows)). Kortix is the open-source AI Operating System: the agents, skills, company memory and connectors live in one git repo you own, and each session runs on its own isolated machine and branch. The questions on this page cover what a migration does to the n8n work you already have, whether both tools can run at once, and what happens to the credentials and secrets involved.

## Do I lose my existing n8n workflows?

No. A migration copies jobs; it does not touch the n8n instance. Your workflows stay on the canvas and keep running, with their execution history and credentials intact, until you choose to disable one. Moving a job means rebuilding it in Kortix as an agent, waiting until it behaves, and only then switching the n8n version off. Because the two run in parallel, a mistake costs you nothing on the n8n side.

## Can n8n and Kortix run side by side?

Yes, and that is the intended path. Start one Kortix agent on a `cron` or `webhook` trigger and leave every fixed flow in n8n. A trigger has exactly two types: `cron` and `webhook`, so an n8n schedule or webhook maps to one trigger. Nothing in n8n has to change for both to run, and a migration moves one job at a time with no cutover date. Keep the deterministic workflows in n8n and move the jobs that need judgment, such as research, a report, or a code fix. The [n8n vs Kortix breakdown](https://n8n-alternative.com/n8n-vs-kortix) covers the two point by point.

## What happens to my n8n credentials and secrets?

n8n stores authentication details inside n8n, and a node references them. Kortix treats credentials differently: connector credentials are brokered server-side and never enter the machine, so an agent gets a token and never the key. You connect the same service as a connector account in the Kortix project, and the agent reaches it through that account. The n8n credential is not imported or exported, and it stays in n8n while the n8n workflow keeps running.

A secret is a per-project credential, such as a model API key, that a session needs but that must not live in the repository. Secrets are encrypted at rest with a key per project, and `kortix.yaml` declares secret names and grants; secret values never appear in the repo ([secrets](https://kortix.com/docs/project/secrets)). Environment is the default exposure, and egress-enforced is experimental. Any model provider with your own keys is supported, so the model key you bring is a secret on the project rather than a file in git.

## What runs after the trigger?

This is the difference worth testing on a real job. n8n executes the nodes you drew, in the order you drew them. A Kortix agent plans its own steps, calls its connectors as tools, and finishes open-ended work. The [workflow to agent mapping](02-workflow-to-agent-mapping.md) names each equivalent, and the [open-source n8n alternative](https://n8n-alternative.com/open-source-n8n-alternative) covers the platform in more depth.

Related reading: [n8n migration questions](https://n8n-alternative.com/faq).
