# Local Supabase Setup Agent Skill

This repository contains an agent skill for AI Coding Assistants (like Cursor, Cline, Claude Code, Antigravity, etc.) to understand how to correctly set up, manage, and troubleshoot a **local Supabase development environment**.

## How to Install

You can install this skill into your AI agent environment using the open [Skills CLI](https://skills.sh):

```bash
# To install the local-supabase-setup skill:
npx skills add https://github.com/Kacper-Hernacki/local-supabase-setup.git --skill local-supabase-setup

# To install the deploy-supabase skill:
npx skills add https://github.com/Kacper-Hernacki/local-supabase-setup.git --skill deploy-supabase
```

## Provided Skills

### `local-supabase-setup`
When installed, your AI agent will automatically know:
- How to initialize and start local Supabase projects correctly (`supabase init`, `supabase start`).
- How to resolve common port conflicts ("Bind for 0.0.0.0:54322 failed") by identifying and stopping other running Supabase containers.
- How to properly configure storage buckets in local migrations.
- How to map the generated local keys to `.env.local` variables (`NEXT_PUBLIC_SUPABASE_URL`, `NEXT_PUBLIC_SUPABASE_ANON_KEY`, `SUPABASE_SERVICE_ROLE_KEY`).
- How to troubleshoot database connection errors directly.

### `deploy-supabase`
When installed, your AI agent will automatically know:
- How to link local projects to remote ones (`supabase link`).
- How to pull or push remote database schemas securely (`supabase db push`, `supabase db pull`).
- How to deploy local configuration settings like email auth templates (`supabase config push`).
- How to deploy and manage secrets for Supabase Edge Functions (`supabase functions deploy`, `supabase secrets set`).
- Best practices for avoiding destructive actions like `db reset` on production databases.
- How to handle migration name conflicts and remote state divergence.

## Structure

```
├── local-supabase-setup/
│   └── SKILL.md
├── deploy-supabase/
│   └── SKILL.md
└── README.md
```

## License

MIT
