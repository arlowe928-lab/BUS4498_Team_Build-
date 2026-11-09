# Sort Shift Coverage Information Task Specification

```yaml
# BASIC INFORMATION
task_id: "T1"
task_name: "Sort Shift Coverage Information"
task_owner: "Manager"

# Agent Inference Configuration
Provider: "Microsoft"
Model: "Copilot"
Role: "Extracts structured shift coverage details from employee messages (first name, last initial, shift date, and shift time) and classifies the reason for coverage into predefined categories (school, personal, family, or other)."
Maximum inference requests per task run: "3"
On inference failure or exhausted limits: "Set status to unresolved, log the failure reason, and escalate the message to the Manager for manual review."
```

## 1. Task Goal

- **Objective:** Copilot reads the message and pulls the relevant information: the employee's first name and last initial, shift date, shift time, and reason for coverage (school, personal, family, or other if the reason doesn't fit the first three). It converts the shift date from MM/DD/YYYY to YYYY-MM-DD so T2 can check it.

## 2. Inbound Inputs

### Input 1

- **Input name:** Employee Shift Coverage Request Message
- **What it contains:** Unstructured or semi-structured text message containing employee identification, shift date, shift start and end times, and the reason the employee needs coverage
- **Source:** Employee, through the Microsoft Teams group chat (messages that begin with "Copilot:")

## 3. Tool Permissions and Boundaries

### Task-Wide Limits

- **Total task timeout:** 30 seconds
- **Maximum tool calls:** 3

### Tool 1

- **Tool name:** Extract-ShiftDetails
- **Tool type:** language-model call
- **Supports these permitted subtasks:** Extract shift entities and classify coverage reason
- **Allowed use:** Read incoming employee message text; parse it and output structured JSON containing employee first name, last initial, shift date (YYYY-MM-DD), shift time, and coverage reason.
- **Prohibited use:** Modifying original messages, sending messages or alerts to employees, accessing employee contact records, or writing to the database directly.
- **Approval required:** None within the allowed use
- **Timeout per call:** 10 seconds
- **Maximum retries per call:** 2
- **Retry conditions and failure response:** Retry on API connection timeout or malformed JSON output with a 2-second backoff interval. If retries are exhausted, log the failure and escalate to the Manager.

Tool-specific and task-wide limits both apply; stop at whichever is reached first. Naming a tool does not authorize uses outside its stated permissions.

## 4. How the Agent Should Reason

### Permitted Subtask 1

- **Subtask name:** Extract shift entities and classify coverage reason
- **Subtask description:** Examines the employee's coverage message; extracts first name, last initial, shift date, and shift time; converts the date to YYYY-MM-DD; and classifies the coverage reason into school, personal, family, or other to produce a structured record.
- **Subtask boundary:** Allowed to read and parse message text using Extract-ShiftDetails. Prohibited from contacting employees, modifying the message text, or writing to persistent storage. Prerequisites: inbound message text must be present. No human approval required.
- **Retry limits:** 2

- **Decision guidance:** After each subtask, use its findings to select the permitted subtask most likely to resolve the most important remaining uncertainty. Do not follow a fixed sequence. If no permitted subtask can make useful progress, stop and hand the case to a person.

## 5. When to Stop or Hand Off to a Human

- **Stop successfully when:** The JSON record contains a value (or an empty value, if the employee left it out) for all five fields, and the date is in YYYY-MM-DD format. Missing fields are passed to T2, which sends the employee a format reply.
- **Hand off early when:** The message asks to cover more than one shift, the inference limit is reached, or the model returns malformed JSON after all retries.
- **Hand off to:** Manager

Stop at the first applicable budget limit or handoff condition. While awaiting review, take no further autonomous action.

## 6. Outbound Deliverable

- **Status:** Completed or escalated to human.
- **Result or recommendation:** The completed result. If escalated before reaching a supported result, write undetermined.
- **Evidence summary:** The most important evidence supporting the result or explaining why no result could be reached.
- **Subtasks performed:** Permitted subtasks completed, including repeated attempts.
- **Unresolved issues:** Remaining uncertainties or questions; use none only if no unresolved issue remains.
- **Handoff note:** Reason for stopping, unresolved questions, and what the reviewer needs to decide; write Not applicable for a completed task.
- **Next task or recipient:** T2 Check Message Format
