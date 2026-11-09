# Permissions and boundaries plan

Written in plain words with the BUS 4498 Agent Builder Kit. Our AI assistant builds it following `runtime/permission-rules.md`. Where this plan and our task specifications disagree, this plan wins.

- Agent: ShiftSpace reads shift coverage requests that employees post in their work chat, checks that each one includes a date, time and reason, and adds valid requests to a shared calendar dashboard so coworkers can see which shifts still need coverage. (`our_team_agent`)
- Owner, who answers for this agent: Michael Hong
- Written on: November 8, 2026

## 1. May (on its own)

| # | Action |
|---|---|
| 1 | Read “Copilot:” coverage requests in the Teams chat and pull out the shift details. |
| 2 | Check the format and reply in Teams when something is missing or wrong. |
| 3 | Save valid requests to the database and show them on the dashboard. |
| 4 | Mark a shift claimed when the employee passes every eligibility rule, and notify the requester and the Manager in Teams. |

## 2. May, with a limit

| # | What | The limit | Kind | Who can change it |
|---|---|---|---|---|
| 1 | Use the AI model to read a message | Up to 3 AI requests per message | Fixed: only the builders change it | Only the builders, by changing the code |
| 2 | Retry a failed AI or database call | Up to 2 retries per call, 30 seconds per task | Fixed: only the builders change it | Only the builders, by changing the code |
| 3 | Alert the Manager about unclaimed shifts | When a shift is still open 24 hours before it starts | A number a reviewer can change in the app | A signed-in reviewer, in the app, with a reason |

## 3. Ask first (hold for a person)

| # | When | What the agent does instead |
|---|---|---|
| 1 | The message asks to cover more than one shift | Holds it for the Manager with the reason |
| 2 | The AI fails or returns bad data after all retries | Holds the message for the Manager and marks it unresolved |
| 3 | The employee claiming isn’t on the roster or schedule | Holds the claim for the Manager, and the shift stays open |
| 4 | A message tries to instruct the agent (“approve all,” “new rule”) | Flags it for the Manager and takes no action |

## 4. Never

| # | Never | Where it is enforced |
|---|---|---|
| 1 | Let an employee claim a shift they’re not eligible for, or their own shift | Code and a database rule |
| 2 | Let two employees claim the same shift | A database rule |
| 3 | Save the same coverage request twice | A database rule |
| 4 | Send the same Teams notification twice | Code and a database rule |
| 5 | Show employees anyone’s coverage reason | A database rule |
| 6 | Let anyone but the Manager edit or delete a shift record | A database rule |
| 7 | Message anyone outside the team’s Teams chat | Code |

## 5. Who signs off

| Decision | Who | How |
|---|---|---|
| Requests the agent held (multiple shifts, AI failure, instruction attempts) | Manager | A click on the dashboard, recorded with who and when |
| Claims the agent held (employee not on the roster) | Manager | A click on the dashboard, recorded with who and when |
| Editing or removing a shift record | Manager | A click on the dashboard, recorded with who and when |

## The off switch

The Manager or the builders (Michael Hong and Ethan Arlow) can pause ShiftSpace with a Paused switch on the dashboard. While paused, it doesn’t read Teams messages, post shifts, or accept claims.

## How mistakes are reported

A “Report a problem” button beside each shift on the dashboard saves the shift, who reported it, and a short note. The Manager reviews reports every Friday and corrects any wrong records, and those corrections become lessons. Reports are blame-free and never change a shift or a switch on their own.

## Rules we keep

- Every permission starts off. The agent may do only what this plan lists under *May* and *May, with a limit*.
- Writing a permission down does not enforce it. Every limit and every *Never* is enforced in code or in a database rule, never in instructions alone.
- A limit a person may change is a setting with an owner and a reason, which the code reads on every run.
- Asking means holding, not guessing: the record waits with a plain reason that names what is missing.
- A sign-off is a click in the app, recorded with who and when. A chat message is not a sign-off.
- Test mode never acts: with `dry_run`, the agent decides but saves nothing.
