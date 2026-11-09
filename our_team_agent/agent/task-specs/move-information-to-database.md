# Move Information to Database Task Specification

## Basic Information

- **Task ID:** T3
- **Task name:** Move Information to Database
- **Task type:** Act
- **Task owner:** Manager

## 1. Task Description

ShiftSpace saves each verified coverage request to the `shift_requests` table in our Supabase (PostgreSQL) database with the status "open."

## 2. Inputs

### Input 1

- **Input name:** Verified Shift Coverage Record
- **Contents and format:** Validated JSON object containing employee first name, employee last initial, shift date (YYYY-MM-DD), shift start and end time, and coverage reason category (school, personal, family, or other)
- **Source:** T2 Check Message Format

- **If a required input is missing or invalid:** Do not complete the database insert, flag the record as rejected, and send the record to the Manager for manual resolution.

## 3. Outputs

### Output 1

- **Output name:** Stored Shift Coverage Record
- **Contents and format:** One row in the `shift_requests` table: shift ID (generated), employee first name, last initial, shift date, start time, end time, coverage reason, status ("open"), and created timestamp.
- **Next task or recipient:** T4 Move Information from Database to Dashboard
- **Complete when:** The database confirms the insert and returns the new shift ID.

A database rule prevents saving the same request twice (same employee, date, and start time).

## 4. Planned Tools

### Tool 1

- **Tool name:** Insert-ShiftRequest
- **Input:** Verified Shift Coverage Record
- **Output:** New shift ID, or an error
- **Implementation Route:** Supabase database insert from a Supabase Edge Function
- **Integration approach:** API key stored in Supabase Secrets
- **Role in this task:** Writes the request to the `shift_requests` table
- **Task timeout:** 10 seconds
- **Maximum retries:** 2
- **Retry only when:** The database connection times out
- **On timeout, exhausted retries, or an error that cannot be retried:** Flag the record and notify the Manager in Teams. A duplicate request is not retried; the employee is told the shift is already posted.
