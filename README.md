# Skill Router

一个给 Codex 用的轻量级元技能：在任务开始时，快速判断**要不要使用 skill**，如果要用，就从已安装 skill 中选择最小、最合适的一组。

它不负责替你写界面、操作设计稿工具或修改游戏引擎项目。它负责先做分流，避免两类问题：

- 明明有合适的 skill，却忘了加载。
- 一口气加载太多 skill，让上下文变重、判断变乱。

## 适合谁

适合已经装了多个 Codex skills 的用户，尤其是同时在使用界面设计、设计稿工具、游戏引擎、技能安装、技能创建、Codex 维护等多种工作流的人。

如果你只有一两个 skill，或者每次都明确知道自己要用哪个 skill，这个项目的收益不大。

## 它解决什么痛点

Codex skill 多了以后，真正麻烦的不是“有没有能力”，而是：

- 什么时候该用 UI/UX 设计类 skill
- 什么时候该用设计稿工具类 skill
- 游戏引擎项目是不是该先用项目侦察/结构分析类 skill
- 安装/创建 skill 应该走哪类系统 skill
- 普通命令、后端逻辑、资料查询是不是根本不需要 skill

Skill Router 把这些选择固化成一个很短的预检规则。

## 主要特性

- **快速判断**：普通请求目标是 2 秒内完成路由判断。
- **只选已安装 skill**：不会擅自联网找 skill，也不会自动安装新东西。
- **最小 skill 集合**：默认 0-1 个，跨领域任务才选择 2-3 个。
- **明确跳过**：简单命令、普通读文件、纯后端逻辑、事实查询等任务会直接不用 skill。
- **适合长期扩展**：你可以按自己安装的 skill 修改 `Installed Skill Map`。

## 当前路由范围

| 场景 | 推荐技能类型 |
| --- | --- |
| 网页、落地页、后台、dashboard、移动 UI | UI/UX 设计类 skill |
| 具体前端样式、组件库、响应式组件 | 前端样式/组件实现类 skill |
| 设计 token、组件规范、CSS variables | 设计系统类 skill |
| 品牌语气、视觉识别、品牌一致性 | 品牌管理类 skill |
| Logo、图标、社交图、综合设计任务 | 通用视觉设计类 skill |
| Banner、广告图、网站 hero 创意图 | Banner/营销素材类 skill |
| HTML 幻灯片、演示文稿 | 幻灯片/演示文稿类 skill |
| 设计稿文件读写、节点创建、变量/组件操作 | 设计稿工具操作类 skill |
| 从页面/代码/描述生成完整设计稿画面 | 设计稿生成类 skill |
| 游戏引擎编辑器或项目工作 | 游戏引擎类 skill，再选择更具体的子技能 |
| 安装现成 skill | 技能安装类 skill |
| 创建或修改 skill | 技能创建类 skill |
| Codex 变慢、本地状态清理、日志/会话维护 | Codex 维护类 skill |

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
git clone https://github.com/schusteramber1491998-afk/skill-router.git C:\Users\Administrator\.codex\skills\skill-router
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
使用 UI/UX 设计类 skill + 前端样式实现类 skill，因为这会改变网页 UI 结构和具体样式。
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
把这个页面写到设计稿工具里
```

路由结果：

```text
使用设计稿工具操作类 skill + 设计稿生成类 skill，因为目标产物是完整设计稿页面。
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

如果你的本机 skill 列表不同，直接编辑 `SKILL.md` 里的 `Installed Skill Map` 和 `Common Routes`。

建议保持这个 skill 很短。它是路由器，不是总控手册；内容越臃肿，越容易违背它“快速选择”的目标。

## 开发与校验

校验 skill 结构：

```powershell
python C:\Users\Administrator\.codex\skills\.system\skill-creator\scripts\quick_validate.py C:\Users\Administrator\.codex\skills\skill-router
```

也可以用本仓库路径校验：

```powershell
python C:\Users\Administrator\.codex\skills\.system\skill-creator\scripts\quick_validate.py .\skill-router
```

## License

MIT
