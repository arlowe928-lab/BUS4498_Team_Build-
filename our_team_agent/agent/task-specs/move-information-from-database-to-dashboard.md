# Move Information from Database to Dashboard Task Specification

## Basic Information

- **Task ID:** T4
- **Task name:** Move Information from Database to Dashboard
- **Task type:** Act
- **Task owner:** Manager

## 1. Task Description

ShiftSpace shows each open shift on the dashboard page in our own app, in a calendar view, so employees can see which shifts need coverage. The coverage reason is not shown to employees; only the Manager can see it.

## 2. Inputs

### Input 1

- **Input name:** Stored Shift Coverage Record
- **Contents and format:** Database record containing shift ID, employee first name, last initial, shift date, start and end time, coverage reason, status, and created timestamp.
- **Source:** T3 Move Information to Database

- **If a required input is missing or invalid:** Log the failure, notify the Manager in Teams, and leave the record off the dashboard until it is fixed.

## 3. Outputs

### Output 1

- **Output name:** Open Shift on Dashboard
- **Contents and format:** Calendar entry on the ShiftSpace dashboard page showing shift date, start and end time, the requesting employee's first name and last initial, an "Open" status tag, and a Claim button.
- **Next task or recipient:** T5 Claim a Shift (employees viewing the dashboard)
- **Complete when:** The dashboard displays the shift as open.

## 4. Planned Tools

### Tool 1

- **Tool name:** Dashboard Page
- **Input:** Rows in `shift_requests` with status "open"
- **Output:** Calendar view of open shifts
- **Implementation Route:** A page in the ShiftSpace web app that reads from our Supabase database
- **Integration approach:** Signed-in employees only; reads through Supabase with row-level rules so employees never see the coverage reason
- **Role in this task:** Displays open shifts and updates when a shift is added or claimed
- **Task timeout:** 10 seconds to load
- **Maximum retries:** 2
- **Retry only when:** The database read times out
- **On timeout, exhausted retries, or an error that cannot be retried:** Show an error message on the page and notify the Manager in Teams
