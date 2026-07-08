# Screenshot to Task

把产品截图、UI 稿、PRD 交互说明，转成可直接执行的前端开发任务清单。

Agent 会全方位理解产品意图——弄清**做什么、怎么做、从哪到哪、新建哪些页面、点什么出什么**——然后在你项目根目录生成 `task/{中文任务名称}.md`，供 OpenSpec、Codex、Cursor Agent 逐条执行。

---

## 解决什么问题

设计稿和 PRD 往往散落在截图里，研发接手时要反复看图、猜交互、漏细节。这个 Skill 把「读图 → 理解 → 拆任务」标准化：

- 截图自动归档，任务文档内可引用
- 开头给出**整体任务预览**，30 秒掌握全貌
- 每条任务带 checkbox，AI 工具可直接勾选执行
- 读图有歧义时**暂停问你**，不瞎猜
- **只写前端**，后端逻辑和部署联调不进任务清单

---

## 产出物

运行后，在你的**项目根目录**生成：

```text
task/
├── 会员积分兑换.md                 # 任务文档（中文命名）
└── screenshot/
    └── 会员积分兑换/               # 与 .md 同名的截图目录
        ├── 01-列表页.png
        └── 02-PRD交互说明.png
```

任务文档以 **§0 整体任务预览** 开篇，随后是分层 checkbox 清单：

| 章节 | 内容 |
|------|------|
| **§0 整体任务预览** | 业务线、产品目标、页面全景、用户路径、点击操作表 |
| §1 路由与菜单 | 新增/改造路由、菜单权限 |
| §2 API 与类型 | 前端需调用的接口与 TS 类型（不含后端实现） |
| §3 常量与枚举 | 状态、列配置、操作矩阵 |
| §4 页面 | 按页面分节，标注**新建** / **改造** |
| §5 组件 | 弹窗、抽屉等 |
| §6 交互与状态流转 | 详细点击操作、状态机 |
| §7 校验与错误处理 | 前端校验、Toast 文案 |
| §8 权限 | 按钮/菜单权限控制 |
| §9 验证清单 | 手动测试步骤 |
| §10 范围外 | 后端逻辑等排除项 |
| **§11 待补充项** | 留给你补充遗漏信息（最后一章，必填） |

完整格式示例见 [`screenshot-to-task/examples/example-task-format.md`](screenshot-to-task/examples/example-task-format.md)。

---

## 核心能力

### 产品意图五问

读图阶段必须回答，并写入 §0：

| 问题 | 说明 |
|------|------|
| **做什么** | 产品目标，用户要完成什么 |
| **怎么做** | 前端用什么页面/组件/交互实现 |
| **从哪里到哪里** | 入口菜单 → 中间页 → 终点的完整路径 |
| **新建什么页面** | 逐页标注新建、改造或复用 |
| **点什么出什么** | 每个按钮 → 跳转 / 弹窗 / 抽屉 / 提交 / 状态变更 |

### 业务线识别

从菜单归属、平台名称、PRD 章节等推断业务线（如「会员营销」「补贴管理」），写入文档头部和 §0。

### 用户确认门禁

交互不清楚、页面归属不确定、PRD 与 UI 冲突时，Agent **暂停列出问题**，等你确认或给出修改方案后才继续。不会自行猜测。

### 仅前端范围

- **会写**：页面、组件、路由、交互、校验、API 消费层
- **不会写**：后端实现、定时任务、部署联调顺序（截图涉及的后端内容记入 §10 范围外）

---

## 安装

```bash
npx skills add https://github.com/zeroanonx/screenshot-to-task --skill screenshot-to-task
```

简写：

```bash
npx skills add zeroanonx/screenshot-to-task --skill screenshot-to-task
```

查看仓库内可用 skill：

```bash
npx skills add https://github.com/zeroanonx/screenshot-to-task --list
```

安装后重新开启 Agent 会话，确保 Skill 被加载。

---

## 使用方式

### 方式一：斜杠命令（推荐）

在目标项目中附上截图，输入：

```text
/screenshot-to-task
```

附带补充说明时：

```text
/screenshot-to-task 请扫描当前代码库，映射到具体文件路径
```

### 方式二：自然语言

```text
使用 screenshot-to-task 分析这些截图，生成前端任务。
```

有代码库、希望任务细化到文件路径时：

```text
使用 screenshot-to-task 分析截图，生成前端任务。请扫描当前代码库映射到具体文件。
```

多张截图、多个独立功能时，会自动拆成多个 `task/*.md`，各带独立截图目录。

生成完成后 Agent 会：

1. 告知文件路径和自动推断的**中文任务名称**
2. 询问是否需要**改名**（同步重命名 `.md` 和截图目录）
3. 提醒填写 **§11 待补充项**

---

## 工作流

```text
截图输入
  │
  ├─ Phase 0  截图归档 → task/screenshot/{任务名}/
  │
  ├─ Phase 1  读图 + 产品意图五问 + 业务线识别
  │             └─ 不确定 → 暂停，等你确认
  │
  ├─ Phase 2  交互建模（用户路径、点击操作表、页面清单）
  │
  ├─ Phase 3  拆解前端任务（§0 预览 → §1～§11）
  │
  ├─ Phase 4  代码库映射（可选，细化到文件路径和代码片段）
  │
  └─ Phase 5  命名落盘 → task/{任务名}.md
```

---

## 与 AI 工具配合

| 工具 | 用法 |
|------|------|
| **Cursor Agent** | 直接读取 `task/*.md` 逐条实现 |
| **Codex** | 将 `task/*.md` 作为任务输入 |
| **OpenSpec** | 手动复制到 `openspec/changes/<name>/tasks.md`，运行 `/opsx:apply` |

Skill 产出独立的 `task/*.md`，不会自动写入 OpenSpec 变更目录。

---

## 适用场景

- 产品/设计给了截图或 PRD 图，需要快速拆成前端任务
- 要把设计输入标准化，方便喂给 AI 编码工具
- 纯 PRD 阶段（无代码库）或已有项目（映射到具体文件）均可
- 管理后台、小程序、H5 等多端截图均支持

---

## 仓库结构

```text
screenshot-to-task/
├── README.md
├── .cursor/
│   └── commands/
│       └── screenshot-to-task.md   # 斜杠命令 /screenshot-to-task
└── screenshot-to-task/
    ├── SKILL.md                      # 主流程（五阶段）
    ├── rules/
    │   ├── task-template.md          # 输出模板与章节结构
    │   ├── extraction.md             # 读图与产品意图提取 checklist
    │   └── standards.md              # 任务生成质量门槛
    └── examples/
        └── example-task-format.md    # 通用格式示例（会员积分兑换）
```

---

## License

MIT
