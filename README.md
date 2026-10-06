# Pulse360 Analysis Agent

An exploratory data analysis (EDA) agent that turns plain-English questions into simple, well-explained SQL analysis. Built with [eve](https://eve.dev) and deployed on Vercel.

Ask a question like *"Which customer segments grew fastest last quarter?"* and the agent breaks it into SQL queries, explains the results with context, and suggests relevant next steps.

## Features

- **Natural language to SQL:** complex questions are simplified into clear analysis queries
- **Readable answers:** results are organised for readability, with depth adapted to the user
- **Connected to your tools** via MCP (Model Context Protocol):
  - **Supabase:** manage and query databases, authentication, and storage
  - **Notion:** search and edit pages and databases
  - **Vercel:** deploy agents and apps, manage projects
- **Multiple channels:** talk to it from Slack or the eve terminal UI

## Architecture

```
agent/
├── agent.ts             # Model and runtime config (deepseek/deepseek-v3.1)
├── instructions.md      # Agent identity, tone, and response guidelines
├── channels/
│   ├── eve.ts           # eve TUI / HTTP channel (Vercel OIDC + local dev auth)
│   └── slack.ts         # Slack channel
└── connections/
    ├── supabase.ts      # Supabase MCP
    ├── notion.ts        # Notion MCP
    └── vercel.ts        # Vercel MCP
```

An eve agent is a directory of files under `agent/`; eve compiles and runs it. To change behaviour, edit `instructions.md`. To add capabilities, add tools, connections, channels, skills, subagents, or schedules under `agent/`.

## Getting started

**Prerequisites**

- Node.js 24.x
- [pnpm](https://pnpm.io)
- A Vercel account (connections authenticate through `@vercel/connect`)

**Install and run**

```bash
pnpm install
pnpm dev        # starts the eve dev server with an interactive TUI
```

**Other scripts**

| Command          | What it does                  |
|------------------|-------------------------------|
| `pnpm dev`       | Run locally with the eve TUI  |
| `pnpm build`     | Build the agent               |
| `pnpm start`     | Run the built agent           |
| `pnpm eval`      | Run agent evals               |
| `pnpm typecheck` | Type-check with `tsc`         |
| `pnpm deploy`    | Deploy to Vercel              |

## Deployment

```bash
pnpm deploy     # runs `eve deploy`
```

`eve deploy` links a Vercel project if needed and deploys to production. See the [eve deployment docs](https://eve.dev/docs/guides/deployment/vercel) for auth and environment variables.

## Authentication note

The eve channel currently uses `placeholderAuth()`, which **does not allow browser requests in production**. Before exposing the agent publicly, replace it with a real auth provider (e.g. Auth.js or Clerk), or use `none()` for a public demo.

## Customising

- **Change personality or purpose:** edit `agent/instructions.md`
- **Swap the model:** edit `agent/agent.ts`
- **Add an integration:** `eve registry search <query>`, then `eve add <item> --non-interactive`

## Tech stack

[eve](https://eve.dev) · [Vercel AI SDK](https://ai-sdk.dev) · TypeScript · Zod · MCP · Supabase · Notion · Slack

## Learn more

- [eve documentation](https://eve.dev/docs)
- [Build an Agent tutorial](https://eve.dev/docs/tutorial/first-agent)
- [eve on GitHub](https://github.com/vercel/eve)
