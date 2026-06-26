# Hook: PRD Completeness Audit

## Objective
Run a strict post-generation check on a newly drafted Product Requirements Document (PRD) to ensure completeness and prevent structural omissions.

## Validation Checklist
You must scan the targeted file text and verify the existence of the following blocks. If any of these are missing, flag the document as **FAILED** and reject it:
1. [ ] A metadata block with a clear project name and launch timeline.
2. [ ] At least one quantifiable Success Metric / KPI containing a clear numerical goal.
3. [ ] A complete Functional Requirements table containing explicit `Req ID` tags and `Priority` levels.
4. [ ] An "Out of Scope" section explicitly naming at least two features or items pushed to later product phases.

## Execution Output
If all checks pass, output exactly: `[VALIDATION SUCCESS]: Document adheres to the corporate PRD specification template.`
If a check fails, output a detailed bulleted list specifying exactly which sections must be rewritten or added.
