# Review and Release Report Task Specification

## Basic Information

- **Task ID:** T05
- **Task name:** Review and Release Report
- **Task type:** Decide
- **Task owner:** Assigned human reviewer, either Purajit Ghosh or Akhil Satti.

## 1. Task Description

The human reviewer evaluates the research evidence and any draft report before deciding whether to approve and release the report, request the one permitted additional research pass, or close the run with an explanation.

The reviewer checks that the report answers the research question, accurately represents the supplied evidence, includes working source references, and clearly identifies uncertainty. A report cannot be released solely because it appears confident or well written.

This task also handles insufficient-evidence handoffs from T03 and drafting failures from T04. It does not place bets or authorize access beyond the research task's existing permissions.

## 2. Inputs

### Input 1

- **Input name:** Matchup Evidence Package
- **Contents and format:** A structured record containing the request ID, requester delivery destination, teams, game date, research question, research-pass number, findings, source links, timestamps, tool failures, and unresolved issues.
- **Source:** T03 Research Matchup, directly for escalations or accompanying the draft from T04 Draft Research Report.

### Input 2

- **Input name:** Draft Research Report
- **Contents and format:** A Markdown report containing the matchup summary, evidence, analysis, uncertainty, source references, and status Draft — Pending Human Review. Required for report approval; it may be unavailable when an earlier task fails.
- **Source:** T04 Draft Research Report.

### Input 3

- **Input name:** Exception Details
- **Contents and format:** A research handoff note or Drafting Exception Record identifying the failure, available evidence, unresolved questions, and reason for escalation. Required when the case arrives through an exception path.
- **Source:** T03 Research Matchup or T04 Draft Research Report.

- **If a required input is missing or invalid:** Do not approve or release the report. Record the missing information and notify the team coordinator. The reviewer may request the one permitted additional research pass if T03 can resolve the problem within its existing permissions. Otherwise, close the run with an explanation. A missing draft is acceptable for reviewing an exception but not for approving a report.

## 3. Outputs

### Output 1

- **Output name:** Approved Research Report
- **Contents and format:** The reviewed Markdown report with the request ID, reviewer name, approval timestamp, version, source references, uncertainty explanation, and status Approved. Include a delivery record identifying the recipient, delivery time, and destination.
- **Next task or recipient:** Research requester, using the delivery destination recorded in the request.
- **Complete when:** The reviewer confirms that the report addresses the research question, checks material factual claims against the supplied sources, records approval, and delivers the approved version to the requester.

### Output 2

- **Output name:** Research Revision Instructions
- **Contents and format:** A structured record containing the request ID, reviewer name, identified issues, specific evidence requiring clarification, and authorization for the one additional research pass.
- **Next task or recipient:** T03 Research Matchup.
- **Complete when:** Clear revision instructions have been handed to T03 and the request record shows that the additional pass has been authorized. This output is permitted only if no additional research pass has already been authorized.

### Output 3

- **Output name:** Review Closure Record
- **Contents and format:** A structured record containing the request ID, status Closed — Unable to Complete or Closed — Review Deadline Expired, the reason, unresolved issues, responsible coordinator or reviewer, closure timestamp, and confirmation that an explanation was delivered.
- **Next task or recipient:** Research requester and team coordinator.
- **Complete when:** The unsuccessful closure is recorded and the requester receives an explanation. No report is labeled approved or successfully completed.

## 4. Planned Tools

### Tool 1

- **Tool name:** review_report
- **Input:** Matchup Evidence Package, Draft Research Report when available, and Exception Details when applicable.
- **Output:** Approved Research Report, Research Revision Instructions, or Review Closure Record.
- **Implementation Route:** File operations through a human-operated document editor and manual handoff through the requester's recorded delivery channel.
- **Integration approach:** Direct integration.
- **Role in this task:** Allow the reviewer to read the report and evidence, inspect cited public sources, record a decision, and manually deliver the approved report or closure explanation. The reviewer may correct wording or formatting without changing the factual meaning. Changes requiring new evidence must follow the authorized research-revision path. Access is limited to the current request, report, evidence, review record, and intended recipient.
- **Task timeout:** Human response deadline of one business day after each review assignment, including a revised report returned for review.
- **Maximum retries:** Not applicable — manual task.
- **Retry only when:** Not applicable.
- **On timeout, exhausted retries, or an error that cannot be retried:** The team coordinator records the unresolved review or delivery error. If the reviewer misses the deadline, the coordinator completes a Review Closure Record and notifies the requester. If delivery fails, retain the approval record but mark delivery as incomplete. Verify whether the recipient received the report before arranging another manual delivery, preventing duplicate releases. If delivery remains impossible, record the failure and close the run without claiming successful delivery.
