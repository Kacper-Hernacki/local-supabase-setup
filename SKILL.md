---
name: local-supabase-setup
description: Comprehensive step-by-step instructions for agents to set up, configure, and troubleshoot a local Supabase environment. This skill should be used whenever you need to initialize Supabase, run migrations, set up storage, or fix local Supabase connection issues.
license: MIT
metadata:
  version: "1.0.0"
---

# Local Supabase Setup Skill

This skill provides the definitive set of instructions for setting up, managing, and troubleshooting a local Supabase development environment. As an AI Agent, use these guidelines when instructed to "set up local Supabase" or when diagnosing connection/configuration issues with a local Supabase instance.

## 1. Prerequisites & Initialization

When setting up a local Supabase environment for a Next.js or React project:

1. **Verify CLI**: Ensure the Supabase CLI is installed. Run `supabase --version`.
2. **Initialize**: If `supabase/config.toml` does not exist, run `supabase init` in the project root.
3. **Link (optional)**: If connecting to an existing remote project, run `supabase link --project-ref <your-project-ref>`.

## 2. Managing the Local Environment

### Starting Supabase
Always ensure no conflicting Supabase instances are running from other projects:
1. `supabase status` - Checks current running instances.
2. If another project is holding ports (e.g., port 54322 is in use), stop it: `supabase stop --project-id <conflicting-project-name>`.
3. Clear stopped containers if necessary: `docker stop $(docker ps -aq)`.
4. Start the instance: `supabase start`.

### Stopping Supabase
- Normal shutdown: `supabase stop`
- Wipe local data and instances: `supabase stop --backup` (or `--no-backup` if data loss is acceptable).

## 3. Database & Migrations

- **Create Migration**: `supabase migration new <migration_name>`
- **Apply Migrations**: `supabase start` applies migrations by default. Alternatively, `supabase db push` (for linked projects) or `supabase migration up`.
- **Reset Database**: `supabase db reset` applies all migrations and seed data. Use this when the schema is out of sync.
- **Seeding Data**: Place initial data in `supabase/seed.sql`. This file runs automatically on `supabase db reset`.

## 4. Storage Setup

To configure storage buckets automatically in a local environment, create an SQL migration (e.g., `supabase/migrations/000_storage.sql`):

```sql
insert into storage.buckets (id, name, public)
values ('bucket-name', 'bucket-name', true)
on conflict (id) do nothing;

create policy "Allow public read access"
on storage.objects for select
using ( bucket_id = 'bucket-name' );

create policy "Allow authenticated uploads"
on storage.objects for insert
with check ( bucket_id = 'bucket-name' and auth.role() = 'authenticated' );
```

## 5. Local Environment Variables

After running `supabase start`, the CLI outputs API URLs and keys. You must update the project's `.env.local` (or equivalent) file:

- `NEXT_PUBLIC_SUPABASE_URL=http://127.0.0.1:54321` (API URL)
- `NEXT_PUBLIC_SUPABASE_ANON_KEY=<anon-key>`
- `SUPABASE_SERVICE_ROLE_KEY=<service-role-key>`

**Note:** For Next.js projects, ensure the prefix `NEXT_PUBLIC_` is used for keys needed on the client-side.

## 6. Accessing the Environment

- **Studio URL**: http://127.0.0.1:54323 (Default port for the UI dashboard)
- **API URL**: http://127.0.0.1:54321
- **DB Password**: Usually `postgres` for local access, running on `127.0.0.1:54322`.

## 7. Troubleshooting Checklists

### "Port is already allocated" / "Bind for 0.0.0.0:54322 failed"
1. Another Supabase instance is running. Find it: `docker ps`.
2. Stop the conflicting project: `supabase stop --project-id <other-project-id>`.

### "Error connecting to database" from App
1. Check `config.toml` to ensure the correct PostgreSQL major version is matching your system or remote project.
2. Verify `.env.local` keys exactly match the output of `supabase status`.

### Magic Link / Email Testing
- Local Supabase captures emails in Inbucket.
- Access it via the local Studio at http://127.0.0.1:54323/project/default/auth/emails or via Inbucket directly if exposed.
