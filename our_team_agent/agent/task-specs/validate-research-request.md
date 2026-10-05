# Validate Research Request Task Specification

## Basic Information

- **Task ID:** T02
- **Task name:** Validate Research Request
- **Task type:** Verify
- **Task owner:** Team Jit coordinator, assigned to Purajit Ghosh or Akhil Satti.

## 1. Task Description

This task applies fixed validation rules to the Research Request supplied by T01 Define Research Request.

The request passes validation when:
- The request ID and requester delivery destination are present.
- Two distinct teams match the configured NFL team list.
- The game date is a valid calendar date.
- The research question is present and identifies a supported analysis category.
- The question does not request bet placement, account access, or guaranteed outcomes.

Supported analysis categories are game-winner analysis, spread analysis, and game-total analysis. T01 records the selected category as a structured field alongside the research question.

The validator uses required-field checks, date parsing, and predefined lists. It does not interpret arbitrary wording using AI or decide which research actions to perform. Validation confirms request completeness and supported scope; T03 confirms matchup details against source evidence.

## 2. Inputs

### Input 1

- **Input name:** Research Request
- **Contents and format:** A structured record containing the request ID, requester delivery destination, two NFL team names, game date, research question, selected analysis category, submission timestamp, and submission status.
- **Source:** T01 Define Research Request.

### Input 2

- **Input name:** Request Validation Rules
- **Contents and format:** A team-maintained configuration containing required fields, accepted date format, NFL team names and aliases, supported analysis categories, and explicit prohibited-action options.
- **Source:** Team Jit, maintained by Purajit Ghosh and Akhil Satti.

- **If a required input is missing or invalid:** Missing or invalid request fields produce Request Correction Feedback for T01. If the request cannot be read or the validation configuration is missing or unusable, record a Validation Exception Record and notify the team coordinator. Do not forward an unvalidated request to T03.

## 3. Outputs

### Output 1

- **Output name:** Validated Research Request
- **Contents and format:** The original request fields plus normalized team names, normalized game date, selected analysis category, validation timestamp, validation status Passed, and results of the fixed checks.
- **Next task or recipient:** T03 Research Matchup.
- **Complete when:** Every required check passes and the validated record is available to T03.

### Output 2

- **Output name:** Request Correction Feedback
- **Contents and format:** A structured message containing the request ID, status Correction Required, failed checks, missing or invalid fields, unsupported selections, and specific corrections needed.
- **Next task or recipient:** T01 Define Research Request and the research requester.
- **Complete when:** The feedback identifies every detected validation issue and is available to T01 for correction of the same request.

### Output 3

- **Output name:** Validation Exception Record
- **Contents and format:** A structured record containing the request ID when available, status Validation Failed — Technical Error, error details, timestamp, attempt count, and available request data.
- **Next task or recipient:** Team coordinator responsible for T02.
- **Complete when:** The coordinator receives the failure record and the request remains blocked from T03 pending resolution.

## 4. Planned Tools

### Tool 1

- **Tool name:** validate_request
- **Input:** Research Request and Request Validation Rules.
- **Output:** Validated Research Request, Request Correction Feedback, or Validation Exception Record.
- **Implementation Route:** Functions/scripts applying deterministic checks and file operations to read inputs and save the validation result. No script is required for this milestone.
- **Integration approach:** Direct integration.
- **Role in this task:** Check required fields, normalize accepted team aliases, parse the game date, and compare structured scope selections against the configured rules. Permissions are limited to reading the current request and validation configuration and writing its validation result. The tool cannot modify the user's research intent, retrieve matchup evidence, place bets, or release reports.
- **Task timeout:** Maximum total elapsed time of 10 seconds, including any retry and waiting interval.
- **Maximum retries:** 1.
- **Retry only when:** A temporary file-read or file-write failure occurs and sufficient time remains. Wait one second before retrying. Use the same request ID and submission timestamp to identify the validation result, and check whether the result already exists before writing again. Do not automatically retry failed field checks or an invalid configuration. If the outcome of a write or handoff is uncertain, notify the coordinator rather than creating duplicate results.
- **On timeout, exhausted retries, or an error that cannot be retried:** Record a Validation Exception Record and hand the case to the team coordinator. Keep the request blocked from T03. The coordinator may restore the required configuration or file access and arrange a new validation run. If the issue cannot be resolved within one business day, the coordinator records closure and delivers an explanation to the requester.
