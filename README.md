# doctor-survey-report-generator

A reusable skill for generating doctor-facing questionnaire analysis reports with:

- fixed chapter structure
- doctor-viewpoint writing rules
- medical/pharmaceutical compliance constraints
- chart generation
- Word report rendering

## Structure

- `SKILL.md`
  - Top-level workflow and triggering rules
- `agents/`
  - UI metadata
- `references/`
  - Section rules, compliance rules, execution rules, expression modules, payload schema
- `scripts/`
  - Questionnaire parsing and report rendering
- `assets/`
  - Reserved for future static templates or visual assets

## Typical Inputs

- Product name
- Region
- Optional time
- Questionnaire spreadsheet or normalized questionnaire table

## Typical Outputs

- `questionnaire.json`
- `report_payload.json`
- `report_draft.md`
- `report_final.md`
- chart images
- `问卷调研分析报告-{{品种}}-医生端-{{地区}}.docx`
- `report_summary.json`

## Scripts

### Parse questionnaire

```bash
python3 scripts/parse_questionnaire.py input.xlsx -o output/questionnaire.json
```

### Render report

```bash
python3 scripts/render_report.py output/report_payload.json --output-dir output/
```

## Notes

- This repository is optimized for doctor-facing questionnaire reports, not patient experience reports.
- The skill is designed to work with fixed structure and controlled wording, while reducing repetitive phrasing through reusable expression modules.
- `assets/` is currently empty by design and reserved for future static cover or docx templates.
