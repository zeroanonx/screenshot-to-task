## 上下文

`screenshot-to-task` 仓库当前承载一个 Agent Skill，但 `SKILL.md` 内容仍是 `create-rules`（分析代码库 → 生成 AI 编码规则）。用户需要将产品截图、UI 稿、PRD 交互说明转化为结构化前端任务 Markdown，写入用户项目根目录 `task/{中文任务名称}.md`，供 OpenSpec、Codex、Cursor 等工具执行。

探索阶段已确认：
- 截图仅为 skill 的通用输入示例，不绑定具体业务域
- 产出独立于 OpenSpec 变更目录
- 任务名称从截图自动推断中文，生成后提醒用户是否修改
- 可选扫描用户代码库，将任务细化到文件路径和代码片段级

## 目标 / 非目标

**目标：**

- 提供完整的 screenshot-to-task skill 工作流（读图 → 建模 → 拆任务 → 落盘）
- 输出格式对齐用户示例（分层 checklist、可追溯来源、验证清单）
- 支持多图、多功能自动拆分多个 `task/*.md`
- 有代码库时可选映射到具体文件路径

**非目标：**

- 不实现 OCR/图像处理脚本（依赖 Agent 视觉理解能力）
- 不自动创建或写入 OpenSpec 变更
- 不保留 create-rules 的 `.cursor/rules` / `AGENTS.md` 生成能力
- 不绑定 subsidy/补贴等示例业务为固定任务

## 决策

### 1. 目录结构

```
screenshot-to-task/
├── SKILL.md                 # 主流程（五阶段工作流）
├── rules/
│   ├── task-template.md     # 输出模板 + 任务粒度标准
│   ├── extraction.md        # 从截图提取什么（字段清单、状态机等）
│   └── standards.md           # 任务生成质量门槛（替换原 CR 规范）
└── examples/
    └── example-task-format.md # 通用格式示例（非具体业务）
```

**理由**：主流程放 SKILL.md，细则拆到 rules/ 避免 SKILL 过长；examples/ 作 golden reference。

**备选**：单文件 SKILL.md 包含全部内容 → 拒绝，不利于维护。

### 2. 产出路径

固定为 `{用户项目根}/task/{中文任务名称}.md`。

**理由**：用户明确要求独立产出、中文命名、便于人工管理和复制到 OpenSpec。

**备选**：写入 `docs/tasks/` 或 `openspec/changes/` → 拒绝，不符合用户约定。

### 3. 任务命名

自动生成规则：
1. 优先取页面标题或菜单末级名称
2. 多张图取最顶层功能名
3. 2～12 汉字，去除文件系统非法字符
4. 重名追加 `-2`、`-3`
5. 生成后固定话术提醒用户是否改名

### 4. 信息优先级

PRD 文字 > UI 标注/箭头说明 > UI 视觉推断；冲突写入「待确认项」。

### 5. 代码库映射（可选 Phase 4）

- 默认：任务到页面/组件/API 职责级（任何项目可用）
- 有代码库：扫描 `package.json`、`src/views/`、`src/api/` 等，细化到文件路径和代码片段
- 继承 create-rules 的探测清单，但不生成 rules 文件

### 6. 多端/多功能拆分

自动识别不同端（小程序/H5/管理后台）或独立功能，默认拆成多个 `task/*.md`；用户明确要求合并时才合并。

## 风险 / 权衡

| 风险 | 缓解 |
|------|------|
| 截图模糊导致字段/交互遗漏 | extraction.md 强制 checklist；未识别项写入「待确认项」 |
| 中文文件名在部分环境有编码问题 | 禁止非法字符；提供改名提醒 |
| 无代码库时任务不够具体 | 模板区分两档粒度；标注 `{待映射}` 占位 |
| SKILL.md 过长 | 细则外置到 rules/，SKILL 只保留流程和门禁 |
| 与 create-rules 功能断裂 | README 说明两个 skill 定位不同；本仓库专注 screenshot-to-task |

## 迁移计划

1. 重写 `screenshot-to-task/SKILL.md` frontmatter 和正文
2. 新增 `rules/task-template.md`、`rules/extraction.md`
3. 替换 `rules/standards.md` 为任务质量标准
4. 新增 `examples/example-task-format.md`
5. 更新根目录 `README.md`
6. 保留 `create-rules/` 目录不动（若存在），本变更只改 `screenshot-to-task/`

无运行时迁移，仅文档/skill 变更。

## 待决问题

- `create-rules/` 目录是否从本仓库移除或拆为独立 repo？（当前提案：不动，仅改版 screenshot-to-task）
- examples 是否需要一份「有代码库映射」的完整示例？（建议后续用真实项目补充）
