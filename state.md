---
next: 处理 code-reviewer 后台审查报告里的确认问题，修完跑 `npm test` 与 `npm run check`
accept: 18 个测试全绿且 tsc 无错误
---
# 进度
- 完成：核心层、端口、git/claude/codex/desk/registry 适配器、内核用例、CLI、render、install、SKILL.md、ARCHITECTURE/SPEC/README — 证据：提交 3fe5591，`npm test` 18/18，`npx tsc` 无错误。
- 完成：`handoff install` 已把 skill 链接进 `~/.claude/skills/handoff`、`~/.codex/skills/handoff`，并在两边注册 SessionStart hook（原配置备份在会话 scratchpad）。
- 进行中：本交接 #1 即首次实地封存；Codex 接手测试未做。
- 未开始：经验库 v5 的 sink 接入（并行会话正在重构 vault，未提交）。

# 决定
- [user] 绿地重写，不继承参考实现代码 [v1.1]
- [proven] 原话编号用 v<交接号>.<序号>，分叉汇合不撞号 — test/core.test.ts、kernel.test.ts 的 fork/join 用例
- [proven] 并发封存不撞号、take 不重复认领 — kernel.test.ts「concurrent seals」「take」用例
- [proven] Windows 上 git restore 会按全局 core.autocrlf=true 写回 CRLF，属正确行为；测试仓库固定 autocrlf=false
- [agent] 交接是经验库的前置入库层：sink 读 [user]→拍板、[proven]→实测，source 引用 handoff:<项目>#<n>
- [open] vault 接入由谁改：等并行会话提交 v5 后由本会话改，或把接入说明交给那个会话

# 下一步
1. 处理审查报告，修复确认的问题并补测试。
2. 用 `codex exec` 在 D:\zuomian\handoff 执行 `$handoff 1`，确认 Codex 侧接手可用。
3. vault v5 提交后，在 `.agents/skills/sink/SKILL.md` 的单条与复盘模式加入 handoff 一手来源。

# 未决问题
- Codex 对 skill 的注入格式未实测，原话过滤可能漏掉 skill 正文（已加 `` 标记）。
- 单次 seal 约 2 秒（Windows 上约 30 次 git 进程），暂不优化。

# 现场
- code-reviewer 子代理在后台审查本仓库，结果回到本会话。
- 并行会话（D:\zuomian\Obisidian 下的 Claude Code）正在把 vault 重构为 v5（30.Wiki、.agents/skills、kb、capture），未提交；不要并行改 vault 文件。
