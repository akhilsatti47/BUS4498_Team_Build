# Draft Research Report Task Specification

## Basic Information

- **Task ID:** T04
- **Task name:** Draft Research Report
- **Task type:** Reason
- **Task owner:** Team coordinator responsible for the report-drafting process.

## 1. Task Description

This task uses AI and a fixed report structure to summarize the evidence supplied by T03 Research Matchup. It explains the matchup factors, identifies conflicting findings, and clearly communicates uncertainty.

The drafting process uses only the supplied evidence. It does not independently retrieve additional information, invent statistics, or place bets. Its output is an unapproved draft requiring review by T05 Review and Release Report.

## 2. Inputs

### Input 1

- **Input name:** Matchup Evidence Package
- **Contents and format:** A structured record containing:
  - Request ID.
  - Requester delivery destination.
  - NFL teams and game date.
  - Research question.
  - Collected statistics, injury updates, betting odds, and weather information where applicable.
  - Source identifiers, source links, publication or update times where available, and retrieval timestamps.
  - Conflicting findings and unresolved information gaps.
  - Research limitations.
  - Research-pass number, identifying an initial or additional research pass.
- **Source:** T03 Research Matchup.

### Input 2

- **Input name:** Report Structure and Drafting Rules
- **Contents and format:** A team-maintained template and fixed instructions requiring sections for the request summary, evidence, matchup analysis, uncertainty, sources, and review status. The rules require source references for factual claims and prohibit unsupported statistics or guaranteed outcomes.
- **Source:** Team Jit, maintained by Purajit Ghosh and Akhil Satti.

- **If a required input is missing or invalid:** Do not generate a report as though the inputs were complete. Record the missing or invalid fields and send a Drafting Exception Record to T05 Review and Release Report. Known evidence gaps explicitly documented by T03 may be summarized as limitations; missing required request details, unusable source references, or a missing drafting template prevent normal drafting.

## 3. Outputs

### Output 1

- **Output name:** Draft Research Report
- **Contents and format:** A Markdown document containing:
  - Request ID and requester delivery destination.
  - NFL matchup, game date, and research question.
  - Report-generation timestamp and research-pass number.
  - Summary of relevant evidence.
  - Analysis addressing the research question.
  - Clear separation between sourced facts and interpretation.
  - Conflicting findings, information gaps, and uncertainty.
  - Source references and retrieval timestamps.
  - Status: Draft — Pending Human Review.
- **Next task or recipient:** T05 Review and Release Report, together with the original Matchup Evidence Package.
- **Complete when:** The document follows the required structure, factual claims reference supplied sources, known limitations are included, and the draft and evidence package are available to T05. Completion of this task does not mean the report has been approved.

### Output 2

- **Output name:** Drafting Exception Record
- **Contents and format:** A structured record containing the request ID when available, status Drafting Failed, the reason for failure, missing fields or error details, attempt count, timestamp, and the available evidence package.
- **Next task or recipient:** T05 Review and Release Report.
- **Complete when:** The failure record and available inputs have been handed to the reviewer without labeling any incomplete draft as approved or ready for release.

## 4. Planned Tools

### Tool 1

- **Tool name:** draft_report
- **Input:** Matchup Evidence Package and Report Structure and Drafting Rules.
- **Output:** Draft Research Report or Drafting Exception Record.
- **Implementation Route:** Functions/scripts calling a language-model web API and saving the result through file operations. This is a planned implementation only; no script is required for this milestone.
- **Integration approach:** Direct integration.
- **Role in this task:** Apply a fixed drafting prompt to the supplied evidence, check the required document sections and source-reference identifiers, and save an unapproved draft. Permissions are limited to reading the supplied inputs and writing the current request's draft or exception record. The tool cannot browse for additional evidence, release reports, or access betting accounts.
- **Task timeout:** Maximum total elapsed time of 60 seconds, including any retry and waiting interval.
- **Maximum retries:** 1.
- **Retry only when:** A temporary API connection failure, rate-limit response, or server error occurs and sufficient time remains within the task timeout. Wait two seconds before retrying, or follow the API's retry interval if it fits within the remaining time. Use the same request ID and research-pass number to identify the draft. Check for an existing completed draft before retrying so duplicate report records are not created. If the outcome of a save is uncertain, hand the case to T05 rather than blindly repeating the write. Missing inputs, unsupported claims, or invalid report content are not eligible for automatic retries.
- **On timeout, exhausted retries, or an error that cannot be retried:** Record a Drafting Exception Record and hand it, the available evidence, and any incomplete draft to T05 Review and Release Report. Keep incomplete drafts marked as failed or pending review. T05 decides whether to request the one permitted additional research pass or close the run with an explanation.
