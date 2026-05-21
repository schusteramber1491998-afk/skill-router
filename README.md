# Skill Router

一个给 Codex 用的轻量级元技能：在任务开始时，快速判断**要不要使用 skill**，如果要用，就从已安装 skill 中选择最小、最合适的一组。

它不负责替你处理文档、检索知识库、研究网页或修改代码。它负责先做分流，避免两类问题：

- 明明有合适的 skill，却忘了加载。
- 一口气加载太多 skill，让上下文变重、判断变乱。

## 适合谁

适合已经装了多个 Codex skills 的用户，尤其是同时在使用文档处理、知识库检索、网页研究、代码项目、浏览器操作、自动化调度、多 Agent 协作等多种工作流的人。

如果你只有一两个 skill，或者每次都明确知道自己要用哪个 skill，这个项目的收益不大。

## 它解决什么痛点

Codex skill 多了以后，真正麻烦的不是“有没有能力”，而是：

- 什么时候该用文档处理类 skill
- 什么时候该用知识库检索类 skill
- 什么时候该用网页研究或浏览器操作类 skill
- 什么时候该用代码项目类 skill
- 什么时候该用自动化、调度或多 Agent 类 skill
- 普通命令、后端逻辑、资料查询是不是根本不需要 skill

Skill Router 把这些选择固化成一个很短的预检规则。

## 主要特性

- **快速判断**：普通请求目标是 2 秒内完成路由判断。
- **只选已安装 skill**：不会擅自联网找 skill，也不会自动安装新东西。
- **最小 skill 集合**：默认 0-1 个，跨领域任务才选择 2-3 个。
- **明确跳过**：简单命令、普通读文件、纯后端逻辑、事实查询等任务会直接不用 skill。
- **适合长期扩展**：你可以按自己安装的 skill 修改 `Installed Skill Categories`。

## 当前路由范围

下面是通用能力分组。第三列给出 Hermes Agent 里同类 skill 的例子，方便你按自己的实际安装列表调整 `SKILL.md`。

| 场景 | 推荐技能类型 | Hermes Agent 示例 |
| --- | --- | --- |
| 复杂任务拆解、长期项目、带文件的计划维护 | 计划管理类 skill | `planning-with-files`、`planner-agent` |
| 本地资料问答、知识库检索、相似资料查找 | 知识库检索类 skill | `kb-retriever`、`semantic-searcher` |
| 知识库整理、资料分组、标签、冲突检查 | 知识库维护类 skill | `kb-curator`、`conflict-detector`、`auto-summarizer` |
| 官方文档、技术资料、库/框架 API 查询 | 文档查找类 skill | `find-docs` |
| 网页搜索、事实核查、竞品/市场资料研究 | 网页研究类 skill | `web-researcher`、`research-agent`、`source-citation-writer` |
| 打开网页、点击路径、表单草稿、页面回放 | 浏览器操作类 skill | `browser-operator` |
| PDF 读取、生成、普通结构分析 | PDF 处理类 skill | `pdf` |
| 扫描件、图片型 PDF、OCR、逐页解析 | 深度 OCR 类 skill | `pdf-ocr-deep` |
| Word 读取、生成、编辑、修订建议 | Word 文档类 skill | `docx`、`docx-editor` |
| PPT 读取、生成、路演/汇报 deck | 演示文稿类 skill | `pptx`、`pptx-builder` |
| Excel/CSV 清洗、公式、透视、统计分析 | 表格分析类 skill | `xlsx`、`xlsx-analyst` |
| 网页、落地页、dashboard、交互原型 | Web 设计实现类 skill | `web-design-engineer` |
| 仓库结构、依赖、入口、模块边界分析 | 代码仓库分析类 skill | `repo-analyzer` |
| 明确 bug、报错、失败原因定位和最小修复 | Bug 修复类 skill | `bug-fixer` |
| 代码审查、安全风险、回归风险、测试缺口 | 代码审查类 skill | `code-reviewer`、`reviewer-agent` |
| 识别并运行测试命令、汇总失败原因 | 测试运行类 skill | `test-runner` |
| 写代码、补丁实现、执行步骤和产物记录 | 执行/编码 Agent 类 skill | `coder-agent`、`executor-agent` |
| 定时任务、重复任务、提醒、长期目标调度 | 调度类 skill | `task-scheduler` |
| 失败重试、降级方案、自动化审计、阶段复盘 | 自动化治理类 skill | `failure-retry`、`automation-auditor`、`milestone-reviewer` |

## 使用方式

把本仓库作为一个 Codex skill 安装到你的 skills 目录。

目录结构：

```text
skill-router/
├── SKILL.md
└── agents/
    └── openai.yaml
```

安装到 Codex 默认位置。

Windows PowerShell：

```powershell
git clone https://github.com/schusteramber1491998-afk/skill-router.git "$env:USERPROFILE\.codex\skills\skill-router"
```

macOS / Linux：

```bash
git clone https://github.com/schusteramber1491998-afk/skill-router.git ~/.codex/skills/skill-router
```

或者手动复制到：

```text
~/.codex/skills/skill-router
```

安装后重启 Codex，让新 skill 进入可发现列表。

## 示例

用户请求：

```text
帮我做一个 SaaS 后台首页，要求看起来专业一点
```

路由结果：

```text
使用 Web 设计实现类 skill，因为这会改变网页 UI 结构和具体样式。
```

用户请求：

```text
帮我查一下这个函数在哪里被调用
```

路由结果：

```text
不需要额外 skill；这是直接的代码搜索任务。
```

用户请求：

```text
分析这个 PDF，提取重点并给我一个整理计划
```

路由结果：

```text
使用 PDF 处理类 skill + 计划管理类 skill，因为任务同时涉及文件解析和后续计划。
```

## 不做什么

Skill Router 不会：

- 主动联网搜索新 skill。
- 自动安装 GitHub 上的陌生 skill。
- 替代领域 skill 的专业能力。
- 为简单任务强行输出长篇计划。
- 绕过系统或开发者指令中对特定 skill 的强制要求。

如果确实需要找新 skill，它会先提示你确认，然后再交给技能安装类 skill 或普通 GitHub 搜索。

## 自定义

如果你的本机 skill 列表不同，直接编辑 `SKILL.md` 里的 `Installed Skill Categories` 和 `Common Routes`。

建议保持这个 skill 很短。它是路由器，不是总控手册；内容越臃肿，越容易违背它“快速选择”的目标。

## 开发与校验

校验 skill 结构：

```powershell
python "$env:USERPROFILE\.codex\skills\.system\skill-creator\scripts\quick_validate.py" "$env:USERPROFILE\.codex\skills\skill-router"
```

也可以用本仓库路径校验：

```powershell
python "$env:USERPROFILE\.codex\skills\.system\skill-creator\scripts\quick_validate.py" .
```

## License

MIT
