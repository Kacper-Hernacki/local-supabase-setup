---
name: deploy-supabase
description: Comprehensive step-by-step instructions for agents to deploy a local Supabase setup to a remote (production/staging) Supabase project. This skill covers linking projects, pushing database migrations, pushing configuration, deploying Edge Functions, and managing secrets.
license: MIT
metadata:
  version: "1.0.0"
---

# Deploy Supabase Skill

This skill provides the definitive set of instructions for taking a local Supabase development environment and deploying it to a remote Supabase project (e.g., staging or production). As an AI Agent, use these guidelines when instructed to "deploy Supabase", "sync local Supabase to production", or "link to a remote Supabase project".

## 1. Prerequisites & Authentication

Before deploying, ensure the CLI is authenticated and the user has a remote project ready.

1. **Login**: Ensure the CLI is authenticated. Run `supabase login`. This will require the user to provide a Personal Access Token from https://supabase.com/dashboard/account/tokens.
2. **Get Project Ref**: Ask the user for the Reference ID of their remote project (a 20-character string found in the project's dashboard URL: `https://supabase.com/dashboard/project/<project-ref>`).

## 2. Linking the Local Project to Remote

To ensure migrations and deployments target the correct remote database:

1. **Link Project**: Run `supabase link --project-ref <project-ref>`.
2. **Database Password**: The CLI will prompt for the remote database password (not the user's account password). Ask the user to enter this when prompted, or pass it via the `SUPABASE_DB_PASSWORD` environment variable.

## 3. Database Migrations

You must push the local schema changes to the remote database securely.

### Step 3.1: Verify Remote Status
Run `supabase db remote commit` to capture any changes made directly in the remote dashboard (like auth settings or manual table changes) that aren't in the local migrations yet.
1. Alternatively, if the remote database is a completely fresh instance, you can skip to pushing.

### Step 3.2: Push Migrations
If the local migrations represent the desired state:
1. Run `supabase db push`.
2. This will apply all pending local migrations in `supabase/migrations/*` to the remote database.

*Note: Never use `supabase db reset` on a production remote database unless explicitly requested by the user, as this drops all data.*

## 4. Deploying Configuration

If the project uses `supabase/config.toml` for settings like Auth, Storage limits, or Webhooks (supported in recent CLI versions):

1. **Push Config**: Run `supabase config push`.
2. This applies local Auth and Storage configurations to the remote project.

## 5. Deploying Edge Functions

If the project uses Supabase Edge Functions (`supabase/functions/*`):

1. **Deploy all functions**: Run `supabase functions deploy`.
2. **Deploy a specific function**: Run `supabase functions deploy <function-name>`.

### Managing Secrets for Functions
Functions often require secrets (like Stripe keys or OpenAI keys) that shouldn't be committed to source control.
1. **Push Secrets**: Create a `.env.production` or `.env` file locally with the required secrets, then run: `supabase secrets set --env-file ./path/to/.env.production`.
2. **List Secrets**: Run `supabase secrets list` to verify.

## 6. Remote Environment Variables Update

Remind the user to update their web application's (e.g., Next.js) remote hosting environment variables (like on Vercel or Netlify) with the new remote project credentials:
- `NEXT_PUBLIC_SUPABASE_URL` -> The remote project URL (e.g., `https://<project-ref>.supabase.co`)
- `NEXT_PUBLIC_SUPABASE_ANON_KEY` -> The remote project's Anon API key
- `SUPABASE_SERVICE_ROLE_KEY` -> The remote project's Service Role key

## 7. Troubleshooting Deployment Iterations

### "Migration name conflicts"
If `supabase db push` fails because a migration has already been applied remotely but with different contents:
1. Run `supabase migration list` to see the sync status.
2. If the remote has drifted, you may need to use `supabase db pull` or `supabase db remote commit` to reconcile changes before generating a new migration with `supabase migration new`.

### Edge Functions failing
Ensure all necessary environment variables have been pushed via `supabase secrets set` before deploying the functions. Functions will fail to start if required keys are missing remotely.
