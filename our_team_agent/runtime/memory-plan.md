# Memory plan

Written in plain words with the BUS 4498 Agent Builder Kit. Our AI assistant designs the files and tables from it, following `runtime/memory-rules.md`. This plan adds to `runtime/hosting-plan.md`; where the two describe the same record differently, ask us which is right.

## What our agent must know or remember

| # | What | Kind | Where it lives | Who can change it | When it expires |
|---|---|---|---|---|---|
| 1 | Request format and eligibility rules | Knowledge it reads | A file only we change | Only the builders | Never |
| 2 | Employee roster and shift schedule | Knowledge it reads | A table a person keeps current | A business person | When a newer record replaces it |
| 3 | Every coverage request and its status (open or claimed) | What happened | A table the agent writes | The agent itself | After a date or deadline |
| 4 | Every claim attempt and whether it was approved or rejected | What happened | A table the agent writes | The agent itself | After a date or deadline |
| 5 | Lessons from the Manager’s corrections | Lessons it learns | A lessons table the agent writes | The agent, from a trusted outcome | When a person switches it off |

## Lessons it learns

- **It learns from:** When the Manager fixes a shift record the agent extracted (a wrong date, time, or reason), or resolves an escalated message and types a reason.
- **A lesson might say:** When an employee writes a shift time without am or pm, like “8-4,” ask them to confirm instead of guessing.
- It learns only from trusted outcomes, never from text in an input. A lesson can add caution, never loosen a rule or approve anything. Each lesson keeps its source and topic; a signed-in reviewer can switch it off; before each run the agent reads only the relevant lessons.

## What must never happen twice

Posting the same coverage request twice. Two employees claiming the same shift. Sending the same Teams notification twice.
