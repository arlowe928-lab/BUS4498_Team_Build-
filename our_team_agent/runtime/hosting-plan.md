# Hosting plan

Written in plain words with the BUS 4498 Agent Builder Kit. Our AI assistant turns it into a GitHub Pages website and a Supabase backend, following `runtime/hosting-rules.md`. Where this plan and our task specifications disagree, this plan wins.

## Our project

- GitHub repository: https://github.com/arlowe928-lab/BUS4498_Team_Build-
- Our agent’s folder: `our_team_agent` (our plans are in `our_team_agent/runtime/`)
- Supabase project: https://zobxtuoinctsljnpsxij.supabase.co
- Owner of the repository and the Supabase project: Ethan Arlow
- Teammate: Ethan Arlow, a collaborator on GitHub and a Developer in the Supabase organization
- We say “I’m working on it” and “pushed” in: Our group chat. One of us works at a time.

## What our agent does

ShiftSpace reads shift coverage requests that employees post in their work chat, checks that each one includes a date, time and reason, and adds valid requests to a shared calendar dashboard so coworkers can see which shifts still need coverage.

## Our tasks

Use the tasks in our task specifications: `our_team_agent/agent/workflow-of-tasks.md` and `our_team_agent/agent/task-specs/`.

## Who can see or change what

Employees can post coverage requests and see every open shift on the dashboard, but they can only claim shifts they’re eligible to work. Managers and the Shift Supervisor see all requests, handle anything the agent flags, and are the only people who can edit or remove a shift record.

## External tools and applications

| # | Tool or application | What the agent uses it for | What it does with it | Most sensitive data it touches | How it connects |
|---|---|---|---|---|---|
| 1 | Microsoft Teams | Reads coverage requests and sends format-error replies to employees | Sends with it | Internal | A sign-in (OAuth) |

## Files people upload

None.
