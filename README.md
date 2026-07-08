# Screenshot to Task

把产品截图、UI 稿、PRD 交互说明，转成可直接执行的前端开发任务清单。

Agent 会全方位理解产品意图，生成**详尽的整体任务预览**，并在你项目根目录输出 `task/{中文任务名称}.md`，供 OpenSpec、Codex、Cursor Agent 逐条执行。

---

## 解决什么问题

设计稿和 PRD 往往散落在截图里，研发接手时要反复看图、猜交互、漏细节。这个 Skill 把「读图 → 理解 → 拆任务」标准化：

- 截图自动归档，任务文档内可引用
- **§0 整体任务预览尽可能详细**，含字段表、全量点击操作、业务规则
- 路由方案**先问你**：新建则确认 path/name，不新建则确认源路径
- 读图有歧义时**暂停问你**，不瞎猜
- **落地前先呈现理解摘要**，问你确认 + 是否还有补充项，**确认后才写文件**
- **只写前端**：不含 API 章节、验证清单、后端逻辑

---

## 产出物

```text
task/
├── 会员积分兑换.md
└── screenshot/
    └── 会员积分兑换/
        ├── 01-列表页.png
        └── 02-PRD交互说明.png
```

| 章节 | 内容 |
|------|------|
| **§0 整体任务预览** | **尽可能详细**：业务线、做什么/怎么做、页面全景、字段表、用户路径、全量点击操作表、状态矩阵、业务规则、权限 |
| §1 路由与菜单 | 项目中怎么配路由（**须先询问用户**是否新建、path/name） |
| §2 常量与枚举 | 状态、列配置、操作矩阵 |
| §3 页面 | 按页面分节，标注新建/改造 |
| §4 组件 | 弹窗、抽屉等 |
| §5 交互与状态流转 | 点击操作实现细节 |
| §6 校验与错误处理 | 前端校验、Toast 文案 |
| §7 权限 | 菜单/按钮权限 |
| §8 范围外 | 后端逻辑等排除项 |
| **§9 待补充项** | 用户填写（最后一章，必填） |

**不包含**：API 与类型、验证清单、部署联调。

示例见 [`screenshot-to-task/examples/example-task-format.md`](screenshot-to-task/examples/example-task-format.md)。

---

## 核心能力

### §0 详尽预览

§0 是整份文档核心，须让读者无需翻截图即可理解需求，包含字段表、逐步展开的用户路径、带条件列的全量点击操作表等。

### 路由先问用户

生成 §1 前必须确认：

- **新建路由** → 严格询问 path、name、菜单位置（禁止自行推断）
- **不新建** → 询问源 path、name、页面文件路径、改造方式

### 产品意图五问

| 问题 | 说明 |
|------|------|
| **做什么** | 产品目标 |
| **怎么做** | 前端实现方案 |
| **从哪里到哪里** | 完整用户路径 |
| **新建什么页面** | 新建/改造标注 |
| **点什么出什么** | 每个按钮的触发结果 |

### 用户确认门禁（全程）

**任何阶段**有不理解、不确定的地方 → 暂停问你 → **你确认后才继续**。禁止自行猜测。

### 落地前确认

写入 `task/*.md` **之前**，Agent 会：

1. 呈现完整理解摘要
2. 询问：理解是否准确？是否还有补充项？
3. **等你明确确认后**，才生成文档（落盘是最后一步）

你当场补充的内容会写入对应章节；表示「后续再补」的留在 §9。

---

## 安装

```bash
npx skills add https://github.com/zeroanonx/screenshot-to-task --skill screenshot-to-task
```

```bash
npx skills add zeroanonx/screenshot-to-task --skill screenshot-to-task
```

安装后重新开启 Agent 会话。

---

## 使用方式

### 方式一：斜杠命令（推荐）

```text
/screenshot-to-task
```

```text
/screenshot-to-task 请扫描当前代码库，映射到具体文件路径
```

### 方式二：自然语言

```text
使用 screenshot-to-task 分析这些截图，生成前端任务。
```

生成流程：读图确认 → 路由确认 → **落地前请你确认理解 + 补充项** → 生成 `task/*.md` → 询问是否改名。

---

## 工作流

```text
Phase 0   截图归档
Phase 1   读图 + 五问（有任何不理解 → 暂停问你）
Phase 2   交互建模
Phase 2.5 路由确认
Phase 3   对话中准备任务（不写文件）
Phase 4   代码库映射（可选）
Phase 5   落地前确认（理解摘要 + 询问补充项 → 等你确认）
Phase 6   写入 task/*.md（最后一步）
```

---

## 与 AI 工具配合

| 工具 | 用法 |
|------|------|
| **Cursor Agent** | `/screenshot-to-task` 或读取 `task/*.md` 逐条实现 |
| **Codex** | 将 `task/*.md` 作为任务输入 |
| **OpenSpec** | 复制到 `openspec/changes/<name>/tasks.md`，运行 `/opsx:apply` |

---

## 仓库结构

```text
screenshot-to-task/
├── README.md
├── .cursor/commands/screenshot-to-task.md
└── screenshot-to-task/
    ├── SKILL.md
    ├── rules/（task-template · extraction · standards）
    └── examples/example-task-format.md
```

---

## License

MIT
