# GKTJ-Dr.-skill

A reusable skill for generating doctor-facing questionnaire analysis reports with:

- fixed chapter structure
- doctor-viewpoint writing rules
- medical/pharmaceutical compliance constraints
- script-built payload JSON from structured intermediate drafts
- chart generation
- explicit Word typography rendering

## Structure

- `SKILL.md`
  - Top-level workflow and triggering rules
- `agents/`
  - UI metadata
- `references/`
  - Section rules, compliance rules, execution rules, expression modules, payload schema
- `scripts/`
  - Questionnaire parsing, payload building, and report rendering
- `assets/`
  - Reserved for future static templates or visual assets; current default renderer does not use a cover page

## Typical Inputs

- Product name
- Region
- Optional time
- Questionnaire spreadsheet or normalized questionnaire table

## Typical Outputs

- `questionnaire.json`
- `report_content.md`
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

### Build payload

```bash
python3 scripts/build_payload.py output/questionnaire.json output/report_content.md -o output/report_payload.json --product "厄贝沙坦氢氯噻嗪片" --region "陕西"
```

### Render report

```bash
python3 scripts/render_report.py output/report_payload.json --output-dir output/
```

## Notes

- This repository is optimized for doctor-facing questionnaire reports, not patient experience reports.
- The skill is designed to work with fixed structure and controlled wording, while reducing repetitive phrasing through reusable expression modules.
- The default workflow is `questionnaire.json -> report_content.md -> report_payload.json -> final artifacts`. Do not ask the model to handwrite a large payload JSON unless you explicitly need the emergency fallback path.
- The default `.docx` output starts from the正文首页 and does not generate a cover page.
- Core typography is explicit rather than style-name-driven: 宋体 20pt/16pt/14pt for heading hierarchy and正文.
