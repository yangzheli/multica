# Multica 内容营销选题规划

---

## 一、愿景与思想领导力（吸引高层决策者和技术领导）

### 1. "Your Next 10 Hires Won't Be Human" — 为什么我们要把 AI Agent 当作正式团队成员
- 核心论点：当前 AI 工具还停留在"copilot"阶段，但未来是 agent 作为独立贡献者加入团队。阐述 Multica 的核心理念：agent 不是工具，是队友。
- 切入点：Gartner 预测 90% 的工程师将从写代码转向 AI 流程编排；Fortune 报道的 "supervisor class" 趋势。
- SEO 关键词：`AI agent team management`, `autonomous coding agents 2026`

### 2. 从 Pair Programming 到 Agent Fleet：软件团队的下一个进化
- 回顾：`autocomplete → copilot → pair programming → autonomous agent → agent fleet`
- 定位 Multica 在这条演进线上的位置
- 引用 Anthropic 2026 Agentic Coding Trends Report 的数据

### 3. 为什么 AI Agent 需要的不是更好的 Prompt，而是一个项目管理系统
- 痛点：当前 agent 使用方式是"一次性 prompt"，没有上下文积累、没有技能复用、没有任务追踪
- 类比：人类工程师也不能只靠 Slack 消息工作，需要 Linear/Jira
- 引出 Multica 的 Skills、Issue 系统、Activity 追踪

---

## 二、技术深度文章（吸引工程师，展示技术实力）

### 4. 我们如何设计 Polymorphic Assignee 模型：让 AI Agent 和人类共享同一个看板
- 深入 `assignee_type + assignee_id` 的设计决策
- 为什么不用单独的"bot"系统，而是让 agent 成为一等公民
- 数据库 schema 设计、migration 演进过程
- 适合发布在：团队博客 + Hacker News

### 5. 构建 Agent 执行沙箱：如何安全地让 AI 在你的代码库里自由操作
- 隔离执行环境设计（`~/multica_workspaces/{workspace-id}/{task-id}/`）
- Skills 注入机制（Claude 用 `.claude/`，Codex 用 `CODEX_HOME`）
- 上下文文件（issue 详情、agent 指令、仓库信息）的写入策略
- 安全边界考虑

### 6. 用 Go + sqlc 构建类型安全的后端：我们为什么放弃了 ORM
- sqlc 从 SQL 直接生成 Go 代码的工作流
- 对比 GORM/ent 等 ORM 的 tradeoff
- 29 个 migration 文件的演进故事
- 这个话题在 Go 社区很有流量

### 7. 实时 WebSocket Hub 设计：如何让 Agent 的每一步操作都实时可见
- workspace-scoped pub/sub 架构
- 非阻塞发送 + 背压处理（慢客户端不阻塞快客户端）
- 前端 WebSocket 状态管理
- 从 "daemon heartbeat" 到 "task message streaming" 的全链路

### 8. Agent Task Queue 设计：从入队到执行的全生命周期
- 状态机：`queued → dispatched → running → completed/failed/cancelled`
- 原子化的 `ClaimTask`（并发安全的"取下一个任务"）
- 每 agent 并发限制的实现
- 事件广播 + WebSocket 推送
- 适合对分布式系统感兴趣的读者

### 9. Daemon 架构深度解析：如何在开发者本地机器上运行可靠的 Agent Runtime
- 多 workspace 监听、运行时注册、CLI 自动检测
- Poll-based task claiming（3s 间隔）+ 心跳机制（15s）
- 热重载配置、自动更新、优雅重启
- 与 cloud runtime 的对比
- 这是 Multica 最独特的技术点之一

---

## 三、实用指南（吸引开发者尝试产品）

### 10. 5 分钟上手：用 Multica 给你的团队加一个 AI 工程师
- 从安装 CLI → 启动 daemon → 创建第一个 issue → 分配给 agent → 看结果
- 截图+命令行展示，降低试用门槛

### 11. 如何编写高效的 Agent Skills：让 AI 真正学会你的代码库
- Skills 的最佳实践：什么该写、什么不该写
- 示例：API 风格指南 skill、测试规范 skill、部署流程 skill
- 对比 `CLAUDE.md` / Codex instructions 的使用方式

### 12. Self-Hosting Multica：从 Docker Compose 到生产级部署
- PostgreSQL 17 + pgvector 配置
- 反向代理（Caddy/Nginx）+ WebSocket 支持
- 环境变量配置指南
- 适合注重数据安全/隐私的企业用户

### 13. Claude Code vs Codex CLI：Multica 如何统一管理不同的 Agent 后端
- 统一的 `Execute()` 接口抽象
- 不同 provider 的 session/streaming 差异
- 如何添加新的 agent provider
- 借势 "Claude Code vs Codex" 的搜索热度

---

## 四、行业洞察（SEO + 引流）

### 14. 2026 AI Coding Agent 工具全景图：从 Copilot 到 Autonomous Teams
- 分类梳理：IDE 插件（Cursor, Windsurf）→ CLI Agent（Claude Code, Codex）→ 平台（Multica, Factory, Devin）
- 各自的定位和 tradeoff
- Multica 的差异化：开源、self-host、agent-agnostic

### 15. 为什么"Vibe Coding"不够用：从 solo hacking 到 production-grade agent workflow
- "Vibe coding" 适合原型，但生产环境需要：任务追踪、代码审查、技能复用、audit trail
- Multica 如何弥补这个 gap

### 16. Open Source AI DevTools 的崛起：为什么越来越多团队选择自托管
- 数据隐私、vendor lock-in、定制化需求
- Multica 的自托管故事
- 适合切入 "open source" 和 "self-hosted" 相关搜索

---

## 五、案例与故事（建立信任）

### 17. 我们是怎么用 Multica 开发 Multica 的（dogfooding 实录）
- 最真实的案例：团队日常如何使用 agent 处理 issue
- 真实的 agent 输出、PR 截图、效率数据
- 这类文章转化率很高

### 18. 一个 3 人团队如何用 6 个 Agent 同时推进功能开发
- `max_concurrent_tasks` 从 1 到 6 的演进
- 并发开发的实际体验和踩坑
- 具体的效率提升数据

---

## 推荐发布优先级

| 优先级 | 选题 | 理由 |
| :--- | :--- | :--- |
| 🔥 P0 | #1 愿景文 | 定义品牌叙事，所有后续内容的基础 |
| 🔥 P0 | #10 快速上手 | 降低试用门槛，直接拉转化 |
| 🔥 P0 | #17 Dogfooding | 最有说服力的案例 |
| P1 | #3 为什么需要项目管理 | 解释核心 why，适合社交传播 |
| P1 | #9 Daemon 架构 | 技术深度 + Hacker News 潜力 |
| P1 | #14 工具全景图 | SEO 流量入口 |
| P2 | #4, #7, #8 技术系列 | 持续产出，建立技术品牌 |