# Check Message Format Task Specification

## Basic Information

- **Task ID:** T2
- **Task name:** Check Message Format
- **Task type:** Verify
- **Task owner:** Manager

## 1. Task Description

ShiftSpace checks whether the shift date, shift time, and reason for coverage follow the required format. This check uses fixed rules in code, not the AI model, so the same message always gets the same result.

Format rules:

- First name and last initial are present.
- Shift date is a real date in YYYY-MM-DD format and is not in the past.
- Shift time has a start and an end time, and the end time is after the start time.
- Reason is one of: school, personal, family, or other.

## 2. Inputs

### Input 1

- **Input name:** Structured Shift Coverage Data
- **Contents and format:** JSON object containing extracted fields: employee first name (string), employee last initial (single character), shift date (YYYY-MM-DD), shift time (start and end time), and coverage reason category (school, personal, family, or other)
- **Source:** T1 Sort Shift Coverage Information

- **If a required input is missing or invalid:** Send a reply in the Teams chat listing what was missing or wrong and showing the correct format (STOP1). The run ends, and the employee's resent message starts a new run at T1.

## 3. Outputs

### Output 1

- **Output name:** Verified Shift Coverage Record
- **Contents and format:** Validated JSON object containing confirmed shift details (employee first name, last initial, shift date, shift start and end time, and coverage category) paired with a validation status flag (isValid: true) and verification timestamp.
- **Next task or recipient:** T3 Move Information to Database
- **Complete when:** All required fields meet the format rules and the validation result is confirmed as valid.

### Output 2

- **Output name:** Format Error Reply
- **Contents and format:** Teams message to the employee listing each field that failed and the correct format.
- **Next task or recipient:** Employee (STOP1)
- **Complete when:** The reply is posted in the Teams chat.

## 4. Planned Tools

### Tool 1

- **Tool name:** Validate-ShiftRecord
- **Input:** Structured Shift Coverage Data from T1
- **Output:** isValid (true or false) and a list of the fields that failed
- **Implementation Route:** Code function (Supabase Edge Function)
- **Integration approach:** Called directly by the workflow after T1
- **Role in this task:** Applies the format rules above to every field
- **Task timeout:** 5 seconds
- **Maximum retries:** 1
- **Retry only when:** The function call times out
- **On timeout, exhausted retries, or an error that cannot be retried:** Log the failure and notify the Manager in Teams

### Tool 2

- **Tool name:** Send-TeamsReply
- **Input:** Format Error Reply text
- **Output:** Message posted in the Teams chat
- **Implementation Route:** Microsoft Teams (Microsoft Graph API)
- **Integration approach:** Microsoft sign-in (OAuth)
- **Role in this task:** Tells the employee how to fix their request
- **Task timeout:** 10 seconds
- **Maximum retries:** 2
- **Retry only when:** The Teams API times out or returns a temporary error
- **On timeout, exhausted retries, or an error that cannot be retried:** Log the failure and notify the Manager
