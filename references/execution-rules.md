# Execution Rules

## Standard Run
1. Parse attachment into `questionnaire.json`.
2. Derive question titles.
3. Draft report text.
4. Build `report_payload.json`.
5. Render markdown, charts, docx, summary.
6. Run final checks.

## Required Checks
- chapter count correct
- chapter order correct
- chart count equals chapter 2 question count
- chart style uniform
- no charts in attachment section
- no unsupported data in prose
- no absolute claims

## Default Output Paths
- `04_outputs/questionnaire.json`
- `04_outputs/report_payload.json`
- `04_outputs/report_draft.md`
- `04_outputs/report_final.md`
- `04_outputs/charts/`
- `04_outputs/问卷调研分析报告-{{品种}}-医生端-{{地区}}.docx`
- `04_outputs/report_summary.json`
