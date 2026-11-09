# Claim a Shift Task Specification

## Basic Information

- **Task ID:** T5
- **Task name:** Claim a Shift
- **Task type:** Act
- **Task owner:** Manager

## 1. Task Description

An employee signs in to the ShiftSpace dashboard and clicks Claim on an open shift. ShiftSpace checks whether the employee is eligible. If they are, it marks the shift claimed and notifies the requesting employee and the Manager in Teams. This check uses fixed rules in code, not the AI model.

Eligibility rules (all must be true):

- The employee is signed in and on the active employee roster.
- The employee is not the person who requested coverage.
- The employee has no other shift that overlaps this shift's time, based on the schedule the Manager keeps in ShiftSpace.
- The shift is still open.

## 2. Inputs

### Input 1

- **Input name:** Claim Request
- **Contents and format:** Shift ID and the signed-in employee's ID
- **Source:** Employee, through the Claim button on the ShiftSpace dashboard

### Input 2

- **Input name:** Employee Roster and Schedule
- **Contents and format:** The `employees` and `schedule` tables in our Supabase database, kept up to date by the Manager
- **Source:** Manager

- **If a required input is missing or invalid:** Do not claim the shift. Show the employee an error message and notify the Manager in Teams.

## 3. Outputs

### Output 1

- **Output name:** Claimed Shift Record
- **Contents and format:** The `shift_requests` row updated with status "claimed," the claiming employee's ID, and the claimed timestamp.
- **Next task or recipient:** END · Complete Workflow
- **Complete when:** The database confirms the update and the Teams notifications are sent to the requesting employee and the Manager.

### Output 2

- **Output name:** Not Eligible Message
- **Contents and format:** Message on the dashboard telling the employee which eligibility rule they didn't meet.
- **Next task or recipient:** Employee (STOP2). The shift stays open.
- **Complete when:** The message is shown.

A database rule only updates a shift if its status is still "open," so two employees can never claim the same shift.

If a shift is still open 24 hours before it starts, ShiftSpace notifies the Manager in Teams.

## 4. Planned Tools

### Tool 1

- **Tool name:** Claim-Shift
- **Input:** Claim Request, Employee Roster and Schedule
- **Output:** Claimed Shift Record or Not Eligible Message
- **Implementation Route:** Code function (Supabase Edge Function)
- **Integration approach:** API key stored in Supabase Secrets
- **Role in this task:** Applies the eligibility rules and updates the shift record
- **Task timeout:** 10 seconds
- **Maximum retries:** 1
- **Retry only when:** The database connection times out
- **On timeout, exhausted retries, or an error that cannot be retried:** Leave the shift open, show the employee an error, and notify the Manager in Teams

### Tool 2

- **Tool name:** Send-TeamsNotification
- **Input:** Claimed Shift Record
- **Output:** Teams messages to the requesting employee and the Manager
- **Implementation Route:** Microsoft Teams (Microsoft Graph API)
- **Integration approach:** Microsoft sign-in (OAuth)
- **Role in this task:** Tells the requester and the Manager who is covering the shift
- **Task timeout:** 10 seconds
- **Maximum retries:** 2
- **Retry only when:** The Teams API times out or returns a temporary error
- **On timeout, exhausted retries, or an error that cannot be retried:** Keep the shift claimed and show the Manager a "notification failed" flag on the dashboard
