<div align="center">

<img src="assets/favicon.svg" width="72" alt="Ernesto">

# ERNESTO

*Let me introduce myself.*

**The AI assistant of the REDS institute · HEIG-VD**

`100% local` · `self-hosted` · `unlimited` · `efficient`

</div>

---

This repo contains the training site for AI4REDS. AI4REDS is the local AI project of the REDS institute of HEIG-VD (a school of the Swiss University of Applied Sciences, HES-SO). The site introduces Ernesto and explains how the AI infrastructure works. It is a set of static HTML pages, served by GitLab Pages.

## What is Ernesto?

Ernesto is the AI assistant of the institute. It is a local AI agent with three parts:

1. **The models** — they run on the institute's own server. The main model is currently Qwen3.8-27B, the compression model is currently Qwen3.5-9B. The team updates the models regularly. Files, code and conversations stay on the institute server. No external cloud.
2. **The harness** — the interface on your machine. You give the task, the agent proposes actions, you approve before it acts.
3. **The configuration and skills** — shared institute-wide in GitLab repositories. The configuration holds the persona (SOUL.md). The skills teach task-specific behavior.

Ernesto executes multi-step tasks with tools: files, terminal, web search. It is a working assistant, not a trivia chatbot. The AI4REDS team maintains the server, the models and the standard configuration. Institute members bring the use cases and share their skills.

## What is in the site?

| Page | Content |
|------|---------|
| `index.html` | Landing page: introduction, trailer, workshop |
| `ernesto-trailer.html` | The trailer: a 1 min 47 canvas animation |
| `workshop.html` | Interactive workshop, 5 tabs: architecture, agent loop, context window, skills and persona, prompts and security |
| `assets/` | Favicon and the logos the site uses |

## Links

- AI4REDS project (wiki and repositories): <https://reds-gitlab.heig-vd.ch/reds-general/ai4reds>
- Workshop tabs: `workshop.html?tab=arch` — also `loop`, `ctx`, `skills`, `sec`
- Force dark mode: `workshop.html?dark`

## Project team

| Name | Role | Work on AI4REDS |
|------|------|-----------------|
| [Mehdi Akeddar](mailto:mehdi.akeddar@heig-vd.ch) & [Guillaume Chacun](mailto:guillaume.chacun@heig-vd.ch) | AI Engineer | Created Ernesto, set up the local AI infrastructure of the institute |
| [Yorick Brunet](mailto:yorick.brunet@heig-vd.ch) | DevOps Engineer | Beta testing, GitLab setup, install scripts, new skills |
