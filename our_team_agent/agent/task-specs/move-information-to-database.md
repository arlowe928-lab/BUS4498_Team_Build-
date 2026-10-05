# Move information to Database Task Specification

## Basic Information

- **Task ID:** T3
- **Task name:** Move information to Database
- **Task type:** Act
- **Task owner:** Manger

## 1. Task Description

Copilot will move the correctly formatted data to a PostgreSQL database

## 2. Inputs

### Input 1

- **Input name:** Verified Shift Coverage Record
- **Contents and format:** alidated JSON object containing employee first name, employee last initial, shift date (YYYY-MM-DD), shift time range, coverage reason category (school, sick, family, or other). 

:
- **Source:** T2 Check Message Format

- **If a required input is missing or invalid:** Do not complete database insert, flag the record as rejected, and route the unverified record back to Shift Supervisor for manual resolution

## 3. Outputs

### Output 1

- **Output name:** [Complete this field]
- **Contents and format:** [Complete this field]
- **Next task or recipient:** [Complete this field]
- **Complete when:** [Complete this field]

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
