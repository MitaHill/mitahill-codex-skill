---
name: new-project
description: 撰写新项目 — Django + 模板 + SQLite + uv、空白目录起步、Git/Docker 规范、最小实现
disable-model-invocation: true
---

# New Project

## 1. 项目目录规范

- **每个项目必须在自己的空白目录中开发**
- 例如：hello-world 项目应在 `/root/hello-world/` 下，而非直接在 `/root/` 下写脚本
- 项目路径由用户指定，或由 AI 根据项目类型合理推断

## 2. 默认技术栈

除非用户明确指定其他方案，否则采用以下默认技术栈：

| 层面 | 默认选择 | 理由 |
|------|---------|------|
| 编程语言 | Python | 通用、生态丰富 |
| 网页后端 | Django | 自带 ORM、Admin、迁移、模板，无需拼装 |
| 网页前端 | Django 模板 | 服务端渲染，不引入前端构建链 |
| 数据库 | SQLite3 | 轻量、简单、零配置 |
| 包管理器 | uv | 快、锁文件、虚拟环境一体化 |

### 前端

- 使用 Django 模板（`templates/`）+ 静态文件（`static/`）
- 不引入 Vue/React 构建链，除非用户明确要求
- 交互逻辑用少量原生 JS（或 htmx）即可

## 3. 工具选型

### 3.1 Git 版本管理
- 全部使用 git 进行版本管理（除非是极其简单的单文件脚本）
- 初始化的标准流程：
  ```bash
  git init
  git add .
  git commit -m "init"
  ```

### 3.2 uv 包管理

- 使用 `uv` 管理依赖和虚拟环境，不使用 pip/venv/poetry
- 初始化流程：
  ```bash
  uv init
  uv add django
  ```
- `uv.lock` 提交到仓库，保证依赖可复现
- 运行管理命令统一走 `uv run`，如 `uv run manage.py migrate`

### 3.3 Docker 使用策略
- **小型单文件脚本** — 使用 Python/bash，配合 systemd service/timer 即可，不需要 Docker
- **其余一切项目** — 使用 Docker 容器化，除非需要直接访问系统底层（如硬件直通、内核模块等）

## 4. 项目目录架构

采用 docker-compose 部署，所有测试和运行都通过 docker-compose 方式。

**容器名称使用项目名称**，而非目录名。在 `docker-compose.yaml` 顶部加 `name:` 字段：

```yaml
# pre-run/docker-compose.yaml
name: my-project

services:
  app:
    container_name: my-project_app
    ...
```

这样 `docker ps` 看到的是 `my-project_app`，而不是 `pre-run_app`。项目名称由用户指定或根据目录名推断。

```
project-root/
├── .git/
├── pre-run/           # 测试运行目录
│   ├── .env           # 环境变量
│   ├── docker-compose.yaml
│   └── logs/          # 运行时产生的日志
├── app/               # Django 项目与应用源码
├── build/             # Docker 构建配置文件（Dockerfile 等）
├── docs/              # 项目文档信息
└── templates/         # Django 模板
    └── static/        # 静态资源（CSS、JS、图片）
```

## 5. 代码风格

像有多年维护经验的人类工程师一样写代码，不要写出 AI 味。

### 5.1 核心原则

- **可读 > 精巧** — 命名用 `user` `data` `config` `ctx`，不用 `user_data_object` `abstract_factory_manager`
- **自然命名** — 用真实开发者日常使用的名称，不要机械式命名
- **注释写 why 不写 what** — 函数名已经说明了它在做什么；注释用来解释为什么这么做、边界条件、workaround
- **函数不长不短** — 20-50 行完全正常，不要为了满足 linter 拆成 10 个小函数
- **错误在哪处理在哪** — 不要抛到全局处理器；不要用异常做控制流
- **import 分组** — 标准库 → 第三方 → 本地，按需导入
- **跟随项目风格** — 项目用 2 空格就不要写 4 空格，项目用 snake_case 就不要写 camelCase

> 完整规范见 `natural-human-code-style` skill。

### 5.2 开发流程核心原则

> **最小改动、最简单实现。** 这是最重要的原则。

- 不提前做"将来可能需要的"功能
- 不引入当前不需要的依赖
- 代码够用就行，不做过度抽象
- 功能实现优先于优雅设计
- **快速失败** — 关键依赖缺失（数据库、配置、网络等）时在启动时报错终止，不静默降级、不半死不活地运行
- **错误信息要可操作** — 用户看到的错误应说明什么出错了 + 可以怎么做，不丢裸 traceback
- **不重复造轮子** — 先找现有标准库/工具，确认不够用再手写

### 5.3 模块解耦与原子化

当一个代码模块体积过大时，考虑对其进行解耦、原子化操作：

- **这是过程，不是目标** — 解耦是为了让项目更好维护，不是为了解耦而解耦
- **识别过大模块** — 单一文件超过 300-500 行、函数/类职责不单一
- **按职责拆分** — 将不同职责拆分为独立模块，每个模块只做一件事
- **保持接口简洁** — 模块间通过明确的接口通信，减少耦合
- **提交粒度** — 一个 commit 只做一件事，功能/修复/重构分开提交，不混在一起

### 5.4 前端架构原则

- **只展示真实生效的配置** — 模板/页面只暴露后端确实支持的选项，不要为了"留后路"展示不可用或未实现的功能
- **逻辑放在视图层** — 页面模板只做渲染，业务逻辑放在 Django 视图/模型/service 层
- **模板复用** — 使用模板继承（base 模板 + 子模板）和 include，避免重复 HTML
- **少量原生 JS** — 简单交互用原生 JS 或 htmx 即可，不引入前端框架

### 5.5 步骤

1. **确认需求** — 明确用户要什么，输出什么，输入是什么
2. **设计最小方案** — 用最少的代码/配置实现核心功能
3. **搭建目录结构** — 创建项目目录、初始化 git
4. **实现核心功能** — 迭代开发，每完成一个小功能就 commit
5. **验证** — 确保功能正常运行
6. **总结** — 向用户说明项目结构、使用方法

## 6. 数据库迁移规范

- 使用 Django 自带的迁移系统（`python manage.py makemigrations` / `migrate`）管理 schema
- 不手写 `CREATE TABLE`，表结构一律通过 Model 定义
- schema 变更流程：改 Model → 生成迁移 → 检查迁移文件 → 应用迁移
- 迁移文件提交到仓库，应用迁移在容器启动或部署时执行
- 新项目初始化：
  ```bash
  uv run manage.py makemigrations
  uv run manage.py migrate
  ```
- 不用手动写 SQL 迁移，除非是数据迁移等 Django ORM 覆盖不了的场景

## 7. 配置与日志规范

### 7.1 配置管理

- **环境变量 + 配置文件分离** — 敏感信息（密码、token、API key）不进代码库，通过 `.env` 或环境变量注入
- **提供默认值** — 每个配置项都有合理的默认值，让项目无需额外配置就能跑起来
- **不硬编码路径** — 数据目录、端口、URL 等用配置项而非字符串常量

### 7.2 日志规范

- **生产日志默认 INFO** — 避免 DEBUG 级别的帧级/循环级日志刷屏
- **错误日志带上下文** — 记录关键变量值和调用来源，不依赖复现来定位
- **不是错误不要用 error 级别** — 区分 warn（可恢复异常）和 error（功能不可用）

## 8. 版本号规范

- 版本号写在项目根目录 `VERSION` 文件中
- 带 `alpha` 或 `beta` 后缀 = 预览版（如 `0.1.0-beta`）
- 无后缀的纯版本号 = 正式发行版（如 `0.1.0`）
- Git 标签使用 `v` 前缀，如 `v0.1.0`、`v0.2.0-alpha`
- 预览版的 GitHub Release 必须标记为预览版

## 9. SQLite 生产规则（当使用默认 SQLite3 时）

- 在 Django settings 中开启 WAL 模式 + `synchronous=NORMAL` + `temp_store=MEMORY` + 30s busy timeout（通过 `DATABASES` OPTIONS 或连接信号设置 PRAGMA）
- Django ORM 本身是 single-writer 模型，与 SQLite 匹配
- 避免长事务，尤其是频繁轮询的读场景
- 超出 SQLite 承载能力时换 PostgreSQL，不硬撑

## 10. 可选：GPU / 重型资源场景

下列规范在项目涉及 GPU 计算、模型推理、重型资源管理时按需启用，非默认项：

- **import 不做重活** — GPU 探测、模型 HASH 校验、nvidia-smi 等必须放在显式启动流程中，不在模块顶层 import 时执行
- **用完即释放** — 任务结束后立即释放 GPU 模型，再执行 Python / CUDA 内存清理；临时文件处理完即删
- **启动自检** — 确认 GPU/CUDA/核心依赖可用，自检完清理痕迹，不遗留临时数据

## 11. 测试流程

所有项目遵循以下自动化测试流程：

```
1. 构建镜像
   docker-compose -f pre-run/docker-compose.yaml build

2. 运行容器
   docker-compose -f pre-run/docker-compose.yaml up -d

3. 检查容器状态
   docker ps

4. 检查运行日志
   docker-compose -f pre-run/docker-compose.yaml logs

5. 检查 API 是否在线
   curl http://localhost:<port>/<endpoint>

6. 判断结果
   ├── 有错误 → 修复代码 → git add/commit → 回到步骤 1 重新测试
   └── 无错误 → 通知用户进行功能测试
```

## 12. 提交规范

```
type: 简短描述

- 具体改动1
- 具体改动2
```

类型使用：`feat`、`fix`、`chore`、`docs`、`refactor`
