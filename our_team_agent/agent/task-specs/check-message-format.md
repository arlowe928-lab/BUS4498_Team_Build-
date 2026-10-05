# Check Message Format Task Specification

## Basic Information

- **Task ID:** T2
- **Task name:** Check Message Format
- **Task type:** Verify
- **Task owner:** Manager

## 1. Task Description

Copilot checks whether the shift date, shift time, and reason for coverage follow the requested message format.

## 2. Inputs

### Input 1

- **Input name:** Structured Shift Coverage Data
- **Contents and format:** JSON object containing extracted fields: employee first name (string), employee last initial (single character), shift date (YYYY-MM-DD), shift time (string/time range), and coverage reason category (school, sick, family, or other)
- **Source:** T1 Sort Shift Coverage Information

- **If a required input is missing or invalid:** Send return message to employee with requested format.

## 3. Outputs

### Output 1

- **Output name:** Verified Shift Coverage Record
- **Contents and format:** Validated JSON object containing confirmed shift details (employee first name, last initial, shift date, shift time range, and coverage category) paired with a validation status flag (isValid: true) and verification timestamp.
- **Next task or recipient:** T3 Move information to Database
- **Complete when:** All required fields match schema criteria and the validation result is confirmed as valid.

## 4. Planned Tools

### Tool 1

- **Tool name:** [Complete this field]
- **Input:** [Complete this field]
- **Output:** [Complete this field]
- **Implementation Route:** [Complete this field]
- **Integration approach:** [Complete this field]
- **Role in this task:** [Complete this field]
- **Task timeout:** [Complete this field]
- **Maximum retries:** [Complete this field]
- **Retry only when:** [Complete this field]
- **On timeout, exhausted retries, or an error that cannot be retried:** [Complete this field]
