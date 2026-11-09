# Permissions and boundaries plan

Written in plain words with the BUS 4498 Agent Builder Kit. Our AI assistant builds it following `runtime/permission-rules.md`. Where this plan and our task specifications disagree, this plan wins.

- Agent: BetBrief validates an NFL matchup research request, gathers relevant team statistics, injury reports, odds, and weather from approved public sources, and drafts a sourced report that highlights conflicting information and uncertainty. A human reviews the report before it is delivered; BetBrief does not place bets. (`our_team_agent`)
- Owner, who answers for this agent: Akhil Satti
- Written on: November 8, 2026

## 1. May (on its own)

| # | Action |
|---|---|
| 1 | Validate complete requests against the approved rules. |
| 2 | Retrieve public matchup information from NFL.com, ESPN.com, and weather.gov; save sourced evidence and draft a report marked Pending Human Review. |

## 2. May, with a limit

| # | What | The limit | Kind | Who can change it |
|---|---|---|---|---|
| 1 | Use only the approved sources. | Allow up to 180 seconds and 8 tool calls per research pass | A number a reviewer can change in the app | A signed-in reviewer, in the app, with a reason |

## 3. Ask first (hold for a person)

| # | When | What the agent does instead |
|---|---|---|
| 1 | Required request details are missing, invalid, or outside supported NFL research. | Hold the request and ask for corrections. If they don’t arrive within one business day, the owner closes it with an explanation. |
| 2 | Material evidence conflicts, required odds can’t be verified, or a research limit is reached. | Hold for the reviewer to approve with limitations, authorize the one extra pass, or close the request. |

## 4. Never

| # | Never | Where it is enforced |
|---|---|---|
| 1 | Place bets, access betting accounts, or handle payment information. | Code |
| 2 | Release a report without recorded human approval. | Code and a database rule |

## 5. Who signs off

| Decision | Who | How |
|---|---|---|
| Releasing a report | Assigned reviewer: Purajit Ghosh or Akhil Satti | Approve in the app; record reviewer, decision, report version, and timestamp. |
| Requesting another research pass or closing a request | Assigned reviewer: Purajit Ghosh or Akhil Satti | Record the decision and reason in the app. |

## The off switch

Akhil Satti or Purajit Ghosh can set the agent to Paused in the app and record a reason. While paused, it does no research, drafting, or delivery. Akhil resumes it after review.

## How mistakes are reported

A person reports an issue using a report problem control linked to the request and report version. Akhil reviews it, records the correction and outcome, and pauses delivery of the affected report while it is being reviewed.

## Rules we keep

- Every permission starts off. The agent may do only what this plan lists under *May* and *May, with a limit*.
- Writing a permission down does not enforce it. Every limit and every *Never* is enforced in code or in a database rule, never in instructions alone.
- A limit a person may change is a setting with an owner and a reason, which the code reads on every run.
- Asking means holding, not guessing: the record waits with a plain reason that names what is missing.
- A sign-off is a click in the app, recorded with who and when. A chat message is not a sign-off.
- Test mode never acts: with `dry_run`, the agent decides but saves nothing.
