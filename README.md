# Local Supabase Setup Agent Skill

This repository contains an agent skill for AI Coding Assistants (like Cursor, Cline, Claude Code, Antigravity, etc.) to understand how to correctly set up, manage, and troubleshoot a **local Supabase development environment**.

## How to Install

You can install this skill into your AI agent environment using the open [Skills CLI](https://skills.sh):

```bash
npx skills add <your-github-username>/local-supabase-setup
```

## What it does

When installed, your AI agent will automatically know:
- How to initialize and start local Supabase projects correctly (`supabase init`, `supabase start`).
- How to resolve common port conflicts ("Bind for 0.0.0.0:54322 failed") by identifying and stopping other running Supabase containers.
- How to properly configure storage buckets in local migrations.
- How to map the generated local keys to `.env.local` variables (`NEXT_PUBLIC_SUPABASE_URL`, `NEXT_PUBLIC_SUPABASE_ANON_KEY`, `SUPABASE_SERVICE_ROLE_KEY`).
- How to troubleshoot database connection errors directly.

## Structure

- `SKILL.md`: The main instruction file and metadata for the agent.

## License

MIT
