# 目标
做 choysun 自己的 handoff：把 Agent 会话封存成编号，任何 Agent（Claude Code、Codex 及未来更强的模型）输入编号即可接手；并作为第二大脑的前置入库层。[v1.1]

# 范围
- 做：CLI `handoff`（prepare/seal/load/take/join/ls/show/sync/hook/install）、一份 Claude Code 与 Codex 共用的 SKILL.md、SessionStart 路由卡、协议规范与夹具、与经验库 sink 的接入点。
- 不做：权限、沙箱、加密、反注入；托管服务；调用模型 API；GUI；MCP 入口（需要时加一个 face 文件）。

# 最终验收标准
- `npm test` 全绿（核心、原话夹具、真实 git 端到端、分层规则）；`npm run check` 通过。
- 在 Claude Code 封存、在 Codex 用 `$handoff <编号>` 接手，Codex 能复述目标与第一步并开始执行。
- 新会话进入有未接续交接的仓库时，自动出现 ≤5 行路由卡。

# 用户的持久要求
- 绿地重写，不参考旧代码；按 2026 最新、最优雅、教科书级的架构写，新技术可以全用，没有历史包袱。[v1.1]
- 面向 GPT-6、Claude Fable 5.1、Claude Opus 5.5 这类超级 Agent 的并行协同极速开发；能随模型变强而变强。[v1.1]
- 不过度考虑风险、不臆想、不做过度防护。[v1.1]
- 评估这个思想是否符合我们的体系，可以做成前置入库工作，把优点提炼进当前整个系统。[v1.1]

# 被否决的方案
- 在参考实现上修补 — 它绑定单一宿主、用压缩次数当触发、从已压缩上下文重建记录 — 用户 [v1.1]
- voice 全局递增序号 — 分叉后两条 lane 撞号 — Agent（改为 v<交接号>.<序号>）
- MCP 入口、Relay Eval 放进 v1 — 目标 Agent 都能跑 shell；评测要先有真实交接数据 — Agent
- 常驻整份 brief 进上下文 — 违背「常驻上下文只放路由」 — Agent（改为 SessionStart 路由卡）
- 把交接包复制进经验库 — 工作记录不进路由（2026-10-07 拍板）— Agent（经验卡只引用 handoff:<项目>#<n>）

# 架构与约定
- 分层：core（纯）→ kernel（用例，无 IO 模块）→ ports ← adapters；faces 只做呈现；src/wire.ts 是唯一组装根；`test/architecture.test.ts` 强制。
- 存储只用 git 引用 `refs/handoff/{seal,wip,claim}/<n>`；编号与认领都是 `update-ref --stdin` 的 CAS。
- Node ≥24 直接跑 TypeScript，只用可擦除语法；零运行时依赖。
- 文档中文，代码与提交信息英文。
