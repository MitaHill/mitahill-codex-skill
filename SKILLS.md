# Codex Skills 索引

此索引只记录个人安装的可维护 skills；系统内置、应用插件和临时会话能力不重复列入。

## 常驻判断

| Skill | 用途 | 使用建议 |
| --- | --- | --- |
| `pragmatic-judgment` | 从目标和近处证据出发，选择最小、可逆且足够的方案 | 遇到开放问题、范围膨胀或分析瘫痪时优先使用 |
| `ponytail` | 实现时遵循 YAGNI、原生优先、最短可行改动 | 代码任务出现过度设计风险时使用 |
| `natural-human-code-style` | 写出自然、务实的生产代码 | 日常编码时按需使用 |

## 实现与测试

| Skill | 触发场景 |
| --- | --- |
| `implement` | 已有 PRD 或 issue，需要实施 |
| `modify-project` | 修改已有项目，要求最小、稳健改动 |
| `reuse-first` | 新增模块、集成或通用能力前，搜索并评估项目内、标准库、包仓库和 GitHub 的可复用方案，再决定引入依赖或编写小模块 |
| `runtime-verification` | 按 Web、桌面、移动端、CLI 或后端选择真实运行验证；图形化程序必须使用浏览器、模拟器或 Computer Use |
| `new-project` | 从空目录创建 Django 项目 |
| `prototype` | 用一次性原型验证设计或状态模型 |
| `tdd` | 用户明确要求测试先行或集成测试 |
| `qa` | 交互式 QA，并将问题记录为 GitHub issue |

## 设计、架构与排障

| Skill | 触发场景 |
| --- | --- |
| `codebase-design` | 模块边界、接口深度、可测试性设计 |
| `design-an-interface` | 需要并列比较多种 API 或模块设计 |
| `improve-codebase-architecture` | 扫描代码库的架构改进点 |
| `diagnosing-bugs` | 难复现 bug、报错、性能退化 |
| `resolving-merge-conflicts` | 正在处理 Git merge 或 rebase 冲突 |
| `server-maintenance` | 服务器环境探查与最小修复 |

## 规划与协作

| Skill | 触发场景 |
| --- | --- |
| `decision-mapping` | 将模糊想法拆为调查事项并逐项收敛 |
| `request-refactor-plan` | 将重构拆成安全的小提交并创建 issue |
| `to-prd` | 将对话整理成 PRD 并发布到 issue tracker |
| `to-issues` | 将计划或 PRD 切成独立、可执行的 issues |
| `triage` | 分类、核验和编写 issue/外部 PR 的执行简报 |
| `handoff` | 将当前任务压缩成交接文档 |

## 审查与约束

| Skill | 触发场景 |
| --- | --- |
| `review` | 审查固定基点以来的变更：规范与需求两条线 |
| `grilling` | 开工前要求严格质询方案或设计 |
| `grill-me` | 同上，适合直接发起质询 |
| `grill-with-docs` | 质询过程中同步产出 ADR 与术语表 |
| `git-guardrails-claude-code` | 为 Claude Code 设置危险 Git 命令保护 |

## Git 与 GitHub

| Skill | 触发场景 |
| --- | --- |
| `git-workflow` | 所有会修改受 Git 跟踪文件的编码任务，以及暂存、提交、分支、合并、变基、推送、PR 或历史恢复；保护已有改动并在 Git 变更前请求精确授权 |
| `git-commit` | 仅根据已暂存的改动，按仓库规范或 Conventional Commits 兜底草拟提交信息；绝不暂存、提交或推送 |
| `git-guardrails-claude-code` | 仅为 Claude Code 安装危险 Git 命令拦截 hook；不约束 Codex |

当前会话已内置 GitHub 相关能力：`github`（仓库、PR、issue 导航）、`gh-address-comments`（处理 PR 审查意见）、`gh-fix-ci`（排查 GitHub Actions），以及 `yeet`（经确认后提交、推送并创建草稿 PR）。无需安装重复的 GitHub CLI 总管 skill。

## 项目初始化与自动化

| Skill | 触发场景 |
| --- | --- |
| `setup-matt-pocock-skills` | 首次为工程 skills 配置 issue tracker、标签和领域文档 |
| `setup-pre-commit` | 配置 Husky、lint-staged、类型检查和测试钩子 |
| `wizard` | 将人工设置或迁移流程变成可交互 Bash 向导 |

## 使用规则

- 同一阶段默认只启用一个主 skill；任务天然跨阶段时组合。例如新增图形模块可按 `reuse-first` → `modify-project` → `runtime-verification` → `review` 执行。
- `ponytail` 约束实现复杂度；`pragmatic-judgment` 约束问题求解范围。两者可同时使用。
- `reuse-first` 约束方案选择，不代表一律引入第三方依赖；`runtime-verification` 约束完成证据，不替代自动化测试。
- `grilling`、`review`、`improve-codebase-architecture` 属于高投入流程，不作为日常小改动的默认步骤。
- 任何第三方 skill 更新或新增前，先审阅其 `SKILL.md` 以及附带脚本。
