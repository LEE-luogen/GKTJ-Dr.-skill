# Report Content And Payload Schema

## Default Workflow
Do not ask the model to handwrite a full `report_payload.json` as the primary path.

Default chain:
1. `scripts/parse_questionnaire.py` -> `questionnaire.json`
2. AI writes `report_content.md` or `report_content.jsonl`
3. `scripts/build_payload.py` -> `report_payload.json`
4. `scripts/render_report.py` -> markdown, charts, docx, summary

This avoids quote escaping failures when the content contains long Chinese paragraphs, Chinese quotation marks, English double quotes, or multiple line breaks.
The default renderer does not create a cover page; it starts directly from the正文首页 with explicit typography rules.

## Recommended `report_content.md` Format

```md
---
product: 厄贝沙坦氢氯噻嗪片
region: 陕西
time: 2025.10
report_type: 医生端问卷调研分析报告
data_issue: 文件名地区与用户输入地区不一致，正文按用户输入输出
unresolved:
---

## 1、引言

第一段。

第二段。

### 1.1 报告背景

第一段。

第二段。

## 2、数据信息分析

### 1. 血压控制达标情况

单题分析正文。

### 2. 长期控制稳定性

单题分析正文。

## 3、反馈意见分析

### 3.1 积极反馈

- 医生认可度较高：正文。
- 长期管理价值较明确：正文。

### 3.2 待改进反馈

- 风险识别标准仍存在差异：正文。
- 患者管理支持仍需加强：正文。

## 4、优化建议

### · 细化重点风险提示

正文。

### · 完善患者教育支持

正文。
```

## Markdown Rules
- `引言` and `报告背景` are paragraph-based sections.
- `数据信息分析` must contain one subheading per questionnaire item, in the same order as `questionnaire.json`.
- `积极反馈` and `待改进反馈` should use `标题：正文` blocks.
- `优化建议` can use either:
  - subheading + body blocks, or
  - `标题：正文` blocks
- `附件-问卷题目内容` should not be authored in the markdown intermediate draft. It is auto-filled from `questionnaire.json`.

## Optional `report_content.jsonl` Format

```jsonl
{"type":"meta","product":"厄贝沙坦氢氯噻嗪片","region":"陕西","time":"2025.10"}
{"type":"introduction","text":"第一段。"}
{"type":"introduction","text":"第二段。"}
{"type":"background","text":"第一段。"}
{"type":"data_analysis","number":1,"title":"血压控制达标情况","analysis":"单题分析正文。"}
{"type":"positive_feedback","title":"医生认可度较高","body":"正文。"}
{"type":"negative_feedback","title":"风险识别标准仍存在差异","body":"正文。"}
{"type":"suggestions","title":"细化重点风险提示","body":"正文。"}
```

## Resulting `report_payload.json`

```json
{
  "product": "厄贝沙坦氢氯噻嗪片",
  "region": "陕西",
  "time": "2025.10",
  "report_type": "医生端问卷调研分析报告",
  "introduction": ["段落1", "段落2"],
  "background": ["段落1", "段落2"],
  "data_analysis": [
    {
      "number": 1,
      "title": "血压控制达标情况",
      "analysis": "正文",
      "question": "原始题目",
      "options": [
        {"label": "A", "text": "选项内容", "pct": "48.98%"},
        {"label": "B", "text": "选项内容", "pct": "43.90%"},
        {"label": "C", "text": "选项内容", "pct": "7.12%"},
        {"label": "D", "text": "选项内容", "pct": "0.00%"}
      ]
    }
  ],
  "positive_feedback": [
    {"title": "标题", "body": "正文"}
  ],
  "negative_feedback": [
    {"title": "标题", "body": "正文"}
  ],
  "suggestions": [
    {"title": "建议标题", "body": "正文"}
  ],
  "attachment_questions": [
    {
      "number": 1,
      "question": "原始题目",
      "options": [
        {"label": "A", "text": "选项内容", "pct": "48.98%"}
      ]
    }
  ],
  "checks": {
    "data_issue": null,
    "unresolved": null
  }
}
```

## Validation Rules
- `data_analysis` item count must equal `questionnaire.json.questions` count.
- `attachment_questions` is auto-generated from `questionnaire.json`.
- `report_type` defaults to `医生端问卷调研分析报告` if omitted.
- `time` may be omitted.
- If a direct JSON payload is used as a fallback path, it must pass `json.loads` validation before rendering.
