# Failure Log

This file captures failure-driven incremental knowledge. Add an entry when an agent makes a wrong assumption, causes a failed test, misses an important rule, or needs a new prevention rule.

## Entry Rules

- Keep each entry short and actionable.
- Record the prevention rule, not just the symptom.
- Do not include secrets, private data, or large raw logs.
- Mark `Status` as `Open`, `Mitigated`, or `Resolved`.

## Log

| Date | Area | Failure | Root Cause | Prevention Rule | Status |
| --- | --- | --- | --- | --- | --- |
| Example | Frontend | Changed layout without checking design rules. | Relevant rule file was not read first. | Read `.agents/rules/frontend_design.md` before frontend UI edits. | Mitigated |
