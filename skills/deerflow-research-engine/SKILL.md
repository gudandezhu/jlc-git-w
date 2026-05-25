---
description: "DeerFlow 调研引擎入口。用法:  <主题> — 启动调研报告生成，自动选择方法论"
allowed_args: "*"
---

# DeerFlow Research Engine

启动 DeerFlow 调研报告生成流程。

## 用法

```
/deerflow-research-engine <调研主题>
```

## Action

用户输入 `/deerflow-research-engine <topic>` 后，按以下流程执行：

### Step 1: 解析参数

从用户输入提取调研主题 `<topic>`。如果未提供主题，用 `AskUserQuestion`（中文提示）要求用户补充。

### Step 2: 选择方法论

使用 `AskUserQuestion`（中文提示）让用户选择调研方法论。示例调用：

```json
{
  "questions": [{
    "question": "请选择调研方法论：",
    "header": "方法论",
    "multiSelect": false,
    "options": [
      {"label": "通用深度调研", "description": "多角度网络调研，适合技术/行业/趋势类主题"},
      {"label": "GitHub 项目调研", "description": "深度分析 GitHub 开源项目，输入格式 owner/repo"},
      {"label": "学术文献综述", "description": "arXiv 论文系统综述，输出 APA/IEEE/BibTeX 引用"},
      {"label": "咨询级研究报告", "description": "麦肯锡/BCG 风格的市场/竞争/行业分析报告"}
    ]
  }]
}
```

### Step 3: 加载对应 Skill

根据用户选择，加载对应的方法论 Skill 文件：

| 用户选择 | 加载的 Skill |
|----------|-------------|
| 通用深度调研 | `skills/deep-research/SKILL.md` |
| GitHub 项目调研 | `skills/github-deep-research/SKILL.md` |
| 学术文献综述 | `skills/systematic-literature-review/SKILL.md` |
| 咨询级研究报告 | `skills/consulting-analysis/SKILL.md` |

### Step 4: 执行调研

按照所选方法论的完整工作流执行调研任务。执行过程中遵循总纲 `skills/deerflow-research-engine/SKILL.md` 中定义的：

- **澄清系统**（CLARIFY → PLAN → ACT）— 所有 AskUserQuestion 使用中文
- **并行编排**（max 3 Agent 并发）
- **引用规范**（inline `[citation:Title](URL)` + Sources section）
- **质量检查**（循环检测、引用核查）
