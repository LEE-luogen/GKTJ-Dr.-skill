# Report Payload Schema

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
    "data_issue": null
  }
}
```

## Notes
- `attachment_questions` should usually mirror normalized questionnaire parsing output.
- `data_analysis[*].options` drive chapter 2 charts.
- `checks.data_issue` can hold known uncertainty, such as a mismatch between user-supplied region and attachment filename.
