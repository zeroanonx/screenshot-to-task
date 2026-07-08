## 为什么

当前 `screenshot-to-task/SKILL.md` 仍是 `create-rules` 的完整副本，无法将产品截图、UI 稿和 PRD 交互说明转化为可执行的前端开发任务。团队需要将设计输入快速整理为结构化 Markdown，供 OpenSpec、Codex、Cursor Agent 等工具直接消费和执行，而现有 skill 不具备截图解析与任务拆解能力。

## 变更内容

- **重写** `screenshot-to-task/SKILL.md`：从「分析代码库生成 AI 规则」改为「分析截图生成前端任务清单」
- **新增** 任务输出规范：`rules/task-template.md`、`rules/extraction.md`
- **新增** 示例文档：`examples/` 目录，展示任务产出格式（不绑定具体业务域）
- **替换** `rules/standards.md`：从 Code Review 规范改为「任务生成质量标准」
- **更新** `README.md`：安装方式、触发语、产出路径说明
- **BREAKING**：移除 create-rules 工作流（目标 AI 选择、`.cursor/rules` 写入等），本 skill 不再生成编码规则文件
- 产出物写入**用户项目根目录** `task/{中文任务名称}.md`，非 OpenSpec 变更目录
- 任务名称由截图内容自动推断为中文，生成后提醒用户是否修改

## 功能 (Capabilities)

### 新增功能

- `screenshot-analysis`：从截图（UI 稿、PRD 文字、流程标注）中提取页面、字段、交互、状态机和业务规则
- `frontend-task-generation`：将解析结果拆解为分层前端任务（路由、API、常量、页面、组件、校验、验证清单）
- `task-output`：按固定模板生成 `task/{中文任务名称}.md`，支持多图多功能自动拆分、可选代码库映射

### 修改功能

（无现有规范）

## 影响

- `screenshot-to-task/SKILL.md` — 全文重写
- `screenshot-to-task/rules/` — 新增/替换规范文件
- `screenshot-to-task/examples/` — 新增示例
- `README.md` — 更新项目说明
- 用户项目 — 运行 skill 时会在根目录创建 `task/` 目录及任务文件
- 与 OpenSpec 关系：skill 产出独立文件，用户可手动复制到 `openspec/changes/*/tasks.md` 或直接用 `/opsx:apply` 执行
