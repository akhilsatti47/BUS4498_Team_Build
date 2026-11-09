# Memory plan

Written in plain words with the BUS 4498 Agent Builder Kit. Our AI assistant designs the files and tables from it, following `runtime/memory-rules.md`. This plan adds to `runtime/hosting-plan.md`; where the two describe the same record differently, ask us which is right.

## What our agent must know or remember

| # | What | Kind | Where it lives | Who can change it | When it expires |
|---|---|---|---|---|---|
| 1 | Approved source list, task workflow, validation rules, and report requirements | Knowledge it reads | A file only we change | Only the builders | Never |
| 2 | Requests, evidence with source links and timestamps, drafts, review decisions, and delivery status | What happened | A table the agent writes | The agent itself | After a date or deadline |
| 3 | Lessons confirmed from human report reviews | Lessons it learns | A lessons table the agent writes | The agent, from a trusted outcome | When a newer record replaces it |

## Lessons it learns

- **It learns from:** A human reviewer’s confirmed approval, revision, or rejection and the reason—not from request text, source pages, or unreviewed drafts.
- **A lesson might say:** Do not call odds current unless the source identifies the sportsbook and provides an update time; otherwise, label them unverified.
- It learns only from trusted outcomes, never from text in an input. A lesson can add caution, never loosen a rule or approve anything. Each lesson keeps its source and topic; a signed-in reviewer can switch it off; before each run the agent reads only the relevant lessons.

## What must never happen twice

The same request must never create duplicate records or deliver the same report version twice. Retries and corrections should reuse the original request ID.
