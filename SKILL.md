---
name: "doctor-survey-report-generator"
description: "Use when generating doctor-facing questionnaire analysis reports from uploaded survey spreadsheets or questionnaire tables, especially when the output must include fixed sections, consistent charts, a Word cover page, and strict medical/pharmaceutical compliance constraints."
---

# Doctor Survey Report Generator

## Overview
This skill generates doctor-facing questionnaire analysis reports with a fixed structure, consistent chart style, and `.docx` output. It is designed for workflows where the user provides a product, region, optional time, and questionnaire data attachment, and expects a reproducible report rather than free-form writing.

## When to Use
- Uploaded attachment contains questionnaire data, especially `.xlsx`, `.csv`, or copied survey tables.
- Output must follow a fixed chapter structure:
  `引言` / `报告背景` / `数据信息分析` / `积极反馈` / `待改进反馈` / `优化建议` / `附件`.
- Every question in chapter 2 needs a matching chart.
- Medical/pharmaceutical tone and doctor viewpoint must stay consistent across the whole report.
- The user wants `.md` and `.docx` artifacts, not just chat output.

Do not use this skill for patient experience reports, clinical trial manuscripts, or unstructured brainstorming.

## Workflow
1. Normalize inputs:
   - Required: `品种`, `地区`, questionnaire attachment.
   - Optional: `时间`, custom execution note, custom output directory.
2. Parse the questionnaire data first.
   - Use `scripts/parse_questionnaire.py` for spreadsheet inputs.
   - Save structured output as JSON before writing any report sections.
3. Derive one statement-style title per question.
   - These titles are the chapter 2 dimension headings and the source for “重点维度”.
4. Generate report content in this order:
   - `引言`
   - `1.1 报告背景`
   - `2、数据信息分析` question by question
   - `3.1 积极反馈`
   - `3.2 待改进反馈`
   - `4、优化建议`
   - `5、附件-问卷题目内容`
   - When drafting prose, use the expression modules in `references/expression-modules.md` to vary:
     - introduction/background openings
     - chapter 2 evidence phrasing
     - chapter 2 clinical interpretation
     - chapter 3 summary wording
     - chapter 4 action verbs
5. Build a report payload JSON.
   - Use the schema in `references/report-payload-schema.md`.
6. Render artifacts.
   - Use `scripts/render_report.py` to generate charts, markdown, docx, and a summary JSON.
7. Verify before delivery.
   - Chart count must equal chapter 2 question count.
   - Attachment must preserve original question and option meaning.
   - No absolute efficacy/safety claims.

## Required Output Rules
- Doctor viewpoint only. Do not rewrite as patient experience.
- Do not invent sample size, hospitals, doctor count, institutions, or dates.
- If sample size or time is missing, use restrained wording and omit unsupported facts.
- In chapter 2, analyze percentage relationships; do not mechanically enumerate A/B/C/D and stop there.
- In chapter 2, always decide the pattern first, then write:
  - consensus-dominant
  - cautious-recognition
  - split-judgement
  - risk-attention
  Use the matching expression modules instead of one fixed paragraph pattern.
- Avoid low-value phrasing such as `A+B很高，C+D很低，因此整体较好`.
- Translate percentage structure into:
  doctor consensus, doctor caution, doctor divergence, clinical management implications.
- Suggestions in chapter 4 must map back to chapter 3.2 findings.
- `引言` and `报告背景` must not be generated from one repeated stock paragraph. Keep the same information structure, but vary expression modules and paragraph emphasis.

## Section Rules
- Read `references/section-rules.md` before generating text.
- Read `references/compliance-rules.md` before finalizing text.
- Read `references/execution-rules.md` before building the report payload.
- Read `references/expression-modules.md` before drafting narrative paragraphs.

## Scripts
- `scripts/parse_questionnaire.py`
  - Reads questionnaire spreadsheets and emits normalized JSON.
- `scripts/render_report.py`
  - Reads a structured report payload JSON and generates:
    - `report_draft.md`
    - `report_final.md`
    - charts
    - `.docx`
    - `report_summary.json`

## Expected File Flow
- Input:
  - attachment spreadsheet
- Intermediate:
  - `questionnaire.json`
  - `report_payload.json`
- Output:
  - `report_draft.md`
  - `report_final.md`
  - `charts/chart_XX.png`
  - `问卷调研分析报告-{{品种}}-医生端-{{地区}}.docx`
  - `report_summary.json`

## Common Mistakes
- Writing the introduction before extracting dimension titles from questions.
- Letting chapter 3 repeat chapter 2 item by item.
- Giving chapter 4 generic advice that does not correspond to chapter 3.2.
- Turning questionnaire feedback into efficacy proof.
- Forgetting that every chapter 2 item needs one chart and only one chart.

## Final Delivery
Reply with:
- markdown final path
- docx path
- chapter completeness check
- chart count check
- chart style consistency check
- missing or uncertain data
- unresolved issues
