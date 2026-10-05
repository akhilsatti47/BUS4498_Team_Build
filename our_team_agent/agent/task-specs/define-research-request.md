# Define Research Request Task Specification

## Basic Information

- **Task ID:** T01
- **Task name:** Define Research Request
- **Task type:** Decide
- **Task owner:** Research requester, with the team coordinator monitoring the response deadline.

## 1. Task Description

The requester manually selects the NFL matchup, game date, and research question. The requester records these details so later tasks can validate the request and research the intended matchup.

If T02 Validate Research Request returns a correction message, the requester reviews the identified issues and updates the same request. This task defines the user's research intent; it does not gather statistics, predict outcomes, or place bets.

## 2. Inputs

### Input 1

- **Input name:** Research Intent
- **Contents and format:** A human response identifying the two NFL teams, game date, research question, and a contact or delivery destination for the result. Selected analysis category: game-winner analysis, spread analysis, or game-total analysis.
- **Source:** Research requester. 

### Input 2

- **Input name:** Request Correction Feedback
- **Contents and format:** A structured message containing the existing request ID, missing or invalid fields, unsupported scope if applicable, and the corrections needed. This input is required only when a request is returned for correction.
- **Source:** T02 Validate Research Request.

- **If a required input is missing or invalid:** The requester supplies or corrects the information before forwarding the request. The task remains paused while awaiting the response. If correction feedback is unclear or unavailable, the team coordinator resolves the issue. If the response deadline expires, the team coordinator closes the run and notifies the requester.

## 3. Outputs

### Output 1

- **Output name:** Research Request
- **Contents and format:** A structured record containing:
  - Unique request ID.
  - Requester contact or delivery destination.
  - Two NFL team names.
  - Game date.
  - Research question.
  - Submission timestamp.
  - Status: Submitted or Resubmitted.
  - Corrections made, when applicable.
  - Selected analysis category.
- **Next task or recipient:** T02 Validate Research Request.
- **Complete when:** The requester has recorded all required fields and forwarded the record to T02. A corrected submission retains the original request ID.

### Output 2

- **Output name:** Request Closure Record
- **Contents and format:** A structured record containing the request ID, status Closed — Requester Response Deadline Expired, outstanding information, closure timestamp, and confirmation that a closure explanation was delivered.
- **Next task or recipient:** Research requester and team coordinator.
- **Complete when:** The team coordinator has recorded the unsuccessful closure and delivered an explanation to the requester. This output applies only when the response deadline expires.

## 4. Planned Tools

### Tool 1

- **Tool name:** record_research_request
- **Input:** Research Intent and, when applicable, Request Correction Feedback.
- **Output:** Research Request.
- **Implementation Route:** File operations through a human-operated editor that saves the structured request record. No script is required for this manual task.
- **Integration approach:** Direct integration through manual file entry and handoff.
- **Role in this task:** The requester enters or edits the research details and saves the record for T02. Access is limited to the current request record; the tool does not retrieve betting data or access betting accounts.
- **Task timeout:** Human response deadline of one business day after the initial request is assigned or a correction message is delivered.
- **Maximum retries:** Not applicable — manual task.
- **Retry only when:** Not applicable.
- **On timeout, exhausted retries, or an error that cannot be retried:** Record the outstanding information or file-entry error and notify the team coordinator. On deadline expiry, the coordinator completes the Request Closure Record and delivers the closure explanation. If saving or forwarding fails, the coordinator verifies whether the existing record was received before arranging another manual handoff. Do not forward an incomplete request as completed or create a duplicate request.
