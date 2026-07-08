## 1. SKILL.md 重写

- [x] 1.1 更新 YAML frontmatter：`name: screenshot-to-task`，重写 `description` 为截图→前端任务场景
- [x] 1.2 删除 create-rules 全部内容：目标 AI 选择门禁、`.cursor/rules` 产出标准、Stylelint 强制项、文件树生成等
- [x] 1.3 编写五阶段工作流：读图分类 → 交互建模 → 任务拆解 → 可选代码库映射 → 命名落盘
- [x] 1.4 写入产出路径硬约束：必须写入 `{项目根}/task/{中文任务名称}.md`
- [x] 1.5 写入任务命名规则：自动推断中文名、非法字符处理、重名序号、生成后提醒用户是否改名
- [x] 1.6 写入多端/多功能拆分规则和输入通用性说明（截图仅为示例，不绑定业务域）
- [x] 1.7 在 SKILL.md 中引用 `rules/task-template.md`、`rules/extraction.md`、`rules/standards.md`

## 2. rules/ 规范文件

- [x] 2.1 新建 `screenshot-to-task/rules/task-template.md`：文档头部模板、固定章节顺序、checkbox 任务格式、有/无代码库两档粒度示例
- [x] 2.2 新建 `screenshot-to-task/rules/extraction.md`：截图分类 checklist（UI稿/PRD/流程/弹窗/表格）、必提取字段清单、状态机/操作矩阵模板、信息源优先级
- [x] 2.3 替换 `screenshot-to-task/rules/standards.md`：从 Code Review 规范改为任务生成质量门槛（可追溯来源、原文保留 Toast/公式、待确认项、验证清单必填）
- [x] 2.4 删除 standards.md 中与 create-rules / CR 相关且不再适用的章节

## 3. examples/ 示例

- [x] 3.1 新建 `screenshot-to-task/examples/example-task-format.md`：通用格式 golden example，使用占位业务名（非补贴等具体域）
- [x] 3.2 示例须展示：需求摘要、分层任务、来源标注、待确认项、验证清单、范围外章节
- [x] 3.3 示例须包含「有代码库映射」和「无代码库 `{待映射}`」各一段对比

## 4. README 更新

- [x] 4.1 重写根目录 `README.md` 标题和定位为 screenshot-to-task skill
- [x] 4.2 更新安装命令：`npx skills add ... --skill screenshot-to-task`
- [x] 4.3 编写触发语示例：「使用 screenshot-to-task 分析这些截图，生成前端任务」
- [x] 4.4 说明产出路径 `task/{中文任务名称}.md` 及与 OpenSpec 的关系（独立产出，可选手动复制）
- [x] 4.5 更新仓库结构说明，反映新的 `rules/` 和 `examples/` 布局

## 5. 验证

- [x] 5.1 通读 `SKILL.md`，确认无 create-rules 残留引用或矛盾指令
- [x] 5.2 确认三份 spec（screenshot-analysis、frontend-task-generation、task-output）均被 SKILL.md 工作流覆盖
- [x] 5.3 用 1～2 张通用 UI 截图试跑 skill（手动），验证能生成符合模板的 `task/*.md` 草稿
- [x] 5.4 验证生成后包含任务名称确认提醒话术
- [x] 5.5 验证多端截图输入时能拆分为多个 `task/*.md` 文件

## 6. 范围外

- [x] 6.1 不修改 `create-rules/` 目录（若存在，保持独立）
- [x] 6.2 不实现 OpenSpec 自动写入集成
- [x] 6.3 不添加 OCR/图像处理脚本
