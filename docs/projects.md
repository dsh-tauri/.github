# dsh-tauri · 项目看板使用指南（HOW WE USE GITHUB PROJECTS）

> 本文件是 dsh-tauri 组织 **GitHub Projects v2** 的落地规范：两个组织级看板 + 配套标签 + 自动化。
> 新维护者/贡献者先读这里。完整调研背景见 <https://github.com/dsh-tauri/.github> 或组织讨论区。

---

## 1. 我们有两块看板，各管一件事

| 看板 | 管什么 | 入口 |
| --- | --- | --- |
| **Issues & PRs** | 当前开发：所有仓库的 issue / PR 追踪与排序 | <https://github.com/orgs/dsh-tauri/projects/2> |
| **Roadmap** | 战略层待办：大功能、文档仓库、大版本发布计划等 | <https://github.com/orgs/dsh-tauri/projects/3> |

设计参考（调研摘要）：
- 组织级**单板多视图**模型（Grafana Alerting 团队），适合多仓库小组织。
- **标签驱动状态流转** + 流程文档化（conda HOW_WE_USE_GITHUB）。
- **里程碑表达决策状态**（VS Code）：Backlog → On Deck → 迭代。
- **Roadmap 用 Draft items + Quarter + 发布 checklist**（docker / github / Electron 发布板）。

---

## 2. 看板 A：Issues & PRs（追踪现状）

**范围**：组织级项目，已链接全部核心仓库（`deepseek-harness-desktop`、`deepseek-harness-desktop-docs`、`deepseek-harness-pkg`、`dsh-pet-mov`、`homebrew-desktop`、`.github` 等；`deepseek-harness-desktop-website` 为私有仓库），任何这些仓库的 issue/PR 都可加进板。

第一方桌面插件**不再是独立仓库**，而是 `deepseek-harness-desktop` 主仓库内 `packages/` 目录下的包：`dsh-tauri`、`dsh-tauri-model-config`、`dsh-tauri-turnrewind`、`dsh-tauri-ui`、`dsh-tauri-worktree`、`dsh-tauri-panel-extension`、`dsh-tauri-panel-scheduler`、`dsh-tauri-session`、`dsh-tauri-pet`、`dsh-tauri-rightclick` 共 10 个，插件相关 issue/PR 请提到主仓库。`dsh-tauri-plugins` 与 `starter-plugin` 均为 fork（前者已归档），不再作为插件主仓库。

### 2.1 字段

| 字段 | 类型 | 取值 | 说明 |
| --- | --- | --- | --- |
| `Status` | Single select | `Triage` · `Backlog` · `In Progress` · `In Review` · `Done` | 主状态列，看板分组依据 |
| `Priority` | Single select | `P0`(紧急) · `P1`(高) · `P2`(普通) · `P3`(低) | 排序/Backlog 视图分组 |
| `Issue Type` | Single select | `bug` · `feature` · `docs` · `chore` | 与 `type/*` 标签对应 |
| `Estimate` | Number | 1/2/3/5/8 | 可选：故事点，做容量规划 |
| `Start date` / `Target date` | Date | — | Roadmap 视图定位（可选） |

> 内置的 `Labels`、`Assignees`、`Milestone`、`Repository`、`Reviewers`、`Linked pull requests` 字段保持默认可用。

### 2.2 视图

| 视图 | 布局 | 用途 |
| --- | --- | --- |
| `Kanban` | Board（按 Status） | **主工作视图**，日常开发 |
| `Triage` | Table（filter Triage） | 维护者给新 issue 排序 |
| `Backlog` | Table（group Priority） | 按优先级看积压 |
| `By Repository` | Table（按 Repository） | 分仓库看板，逐仓库跟进 |
| `Roadmap` | Roadmap（日期） | 排期预演（可选） |

### 2.3 状态流转（手动 + 自动化）

```
新 issue/PR ──► Triage ──► Backlog ──► In Progress ──► In Review ──► Done
                                                     ▲                │
                                                     └── reopened ────┘
```

- **新 issue 进板**：默认全部进 `Triage`（可以在看板 GUI 的 Workflows 里配 Auto-add；或用下面的标签规则手动拖入）。
- **开始做**：拖到 `In Progress`（同时打 `status/backlog` 之外自己想用的标签）。
- **PR 审查**：`In Review`；**合并/关闭** → `Done`。
- **重新打开**：回到 `Triage`。
- **状态与标签双向同步**：置 `Status=Triage/Backlog` 时打 `status/triage` / `status/backlog` 标签，标签也可反向驱动（conda 式）。

### 2.4 配套标签（组织级统一）

**type（4 个，代替默认的 enhancement）：**
- `type/bug` · `type/feature` · `type/docs` · `type/chore`

**status（3 个，与看板状态同步）：**
- `status/triage` · `status/backlog` · `status/blocked`

**保留默认：** `help wanted` · `good first issue` · `question` · `duplicate` · `upstream`

> 给新 issue 至少打一个 `type/*` 标签；看板 `Issue Type` 字段与此对应。

---

## 3. 看板 B：Roadmap（记录待办）

**性质**：组织级 Projects v2，以 **Draft items（草稿卡）为主** —— 不依赖 issue，维护者随手记；大功能成熟后再转成 issue 或另立 roadmap 仓库。

### 3.1 字段

| 字段 | 类型 | 取值 | 说明 |
| --- | --- | --- | --- |
| `Status` | Single select | `Idea` · `Planned` · `In Progress` · `Shipped` | 生命周期 |
| `Quarter` | Single select | `2025-Q4` ~ `2027-Q1` · `Future` | 目标季度（Quarter 视图分组） |
| `Priority` | Single select | `P0`~`P3` | 优先级 |
| `Start date` · `Target date` | Date | — | Roadmap timeline 画时间条 |

> 可选：`Link`（Text 字段）放关联 issue / PR / Discussion 的 URL。

### 3.2 视图

| 视图 | 布局 | 用途 |
| --- | --- | --- |
| `Roadmap` | Roadmap（Start–Target date） | 时间轴总览 |
| `Kanban` | Board（按 Status） | 日常待办操作 |
| `Quarter` | Table（group Quarter） | 按季度排期 |

### 3.3 预置待办（草稿卡，维护者可改）

- **🚀 v1.0 发布计划**：0.x 冻结 → alpha → beta → stable → 发布公告（checklist 用草稿卡正文列任务）。
- **📚 文档仓库内容整理**：`deepseek-harness-desktop-docs` 站点/README 结构化。
- **🧩 大功能候选**：插件签名校验、多 Profile 管理、自动更新推送等（待填）。
- **🏠 组织规范**：全局 labels（本文件 2.4）+ 流程文档化。

---

## 4. 自动化（Workflows）

GitHub Projects v2 内置 Workflows 可在看板 GUI 配置（`… → Workflows`），推荐三条：

1. **Auto-add to project**：新 issue/PR 带 `type/*` 或 `bug`/`enhancement` 标签 → 自动入板（`Triage`）。
2. **Item closed**：issue 关闭 / PR merged → `Status = Done`。
3. **Item reopened**：重新打开 → `Status = Triage`。

> 说明：内置 Workflows 的创建目前只能在 GUI 完成（GraphQL 仅有 `deleteProjectV2Workflow` 等基础操作）；
> 若需要完全代码化的自动化，可用 GitHub Actions + 项目 API 脚本（本项目暂未启用，避免复杂度）。

---

## 5. 里程碑（Milestone）与发布

- 组织仓库继续用 **Milestone 表达发布计划**（如 `v0.9`、`v1.0`）：Backlog → On Deck → 迭代。
- 对应看板 B 的「🚀 v1.0 发布计划」草稿卡，发布时打勾更新。
- 社区大功能排期（可选第二阶段）：启用主仓库 Discussions 的 `💡 Ideas` 分类做收集 + upvote，成熟后转 issue 进看板 A。

---

## 6. 一分钟上手（维护者）

1. 打开 [Issues & PRs](https://github.com/orgs/dsh-tauri/projects/2) 的 `Triage` 视图，把新 issue 按 `Issue Type` + `Priority` 归位。
2. 需要长期跟踪的大功能 / 发布计划 → 记到 [Roadmap](https://github.com/orgs/dsh-tauri/projects/3)（草稿卡即可）。
3. 每个 issue 至少打一个 `type/*` 标签；看板状态尽量与 `status/*` 标签保持一致。
4. 完成后拖到 `Done`（或等 Workflow 自动置 Done）。