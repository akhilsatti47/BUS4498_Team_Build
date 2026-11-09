# Hosting plan

Written in plain words with the BUS 4498 Agent Builder Kit. Our AI assistant turns it into a GitHub Pages website and a Supabase backend, following `runtime/hosting-rules.md`. Where this plan and our task specifications disagree, this plan wins.

## Our project

- GitHub repository: https://github.com/akhilsatti47/BUS4498_Team_Build
- Our agent’s folder: `our_team_agent` (our plans are in `our_team_agent/runtime/`)
- Supabase project: https://uamorxhlthpqcynrdxrd.supabase.co
- Owner of the repository and the Supabase project: AKhil
- Teammate: Purajit, a collaborator on GitHub and a Developer in the Supabase organization
- We say “I’m working on it” and “pushed” in: Our Group Chat.. One of us works at a time.

## What our agent does

BetBrief validates an NFL matchup research request, gathers relevant team statistics, injury reports, odds, and weather from approved public sources, and drafts a sourced report that highlights conflicting information and uncertainty. A human reviews the report before it is delivered; BetBrief does not place bets.

## Our tasks

Use the tasks in our task specifications: `our_team_agent/agent/workflow-of-tasks.md` and `our_team_agent/agent/task-specs/`.

## Who can see or change what

Requesters can view their own requests and delivered reports. The assigned Team Jit coordinator can access that request’s evidence and draft; only the assigned human reviewer can approve or release it. Research tools read approved public sources only and cannot access betting accounts or place bets.

## External tools and applications

| # | Tool or application | What the agent uses it for | What it does with it | Most sensitive data it touches | How it connects |
|---|---|---|---|---|---|
| 1 | NFL.com | Reads public stats and injury reports | Reads from it | Public | Not sure yet |
| 2 | ESPN.com | Reads public stats, injury reports, and odds | Reads from it | Public | Not sure yet |
| 3 | weather.gov | Reads public forecasts for the game location and time | Reads from it | Public | Not sure yet |

## Files people upload

None.
