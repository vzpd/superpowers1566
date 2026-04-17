# CHANGELOG · superpowers1566 分叉

本文件只记录 **superpowers1566 分叉层** 的变更（换皮、叙事、元数据）。
来自上游 [obra/superpowers](https://github.com/obra/superpowers) 的 skill 主体变更记录在 `RELEASE-NOTES.md` 中，与上游同步。

新条目放在**顶部**（最新的在上），格式照抄下方骨架；不要照搬上游 `RELEASE-NOTES.md` 的格式——那是上游叙事，口径不同。

## 条目格式骨架

```markdown
## v<本分叉版本> — <YYYY-MM-DD>

**Fork 基线：** 上游 superpowers `v<上游版本>`（commit `<短 sha>`, <上游 commit 日期>）

### 新增（Added）
- `<文件或路径>` —— 一句话说清做了什么、为什么。

### 修改（Changed）
- `<文件>`：一句话说清变动。

### 未改动（Deliberately unchanged）
- 说明本次哪些看似该动的地方刻意没动、为什么（通常 = "上游 vendored、留给同步"）。

### 已知边界
- 本次遗留 / 下次再处理 / 使用时注意事项。
```

每条至少填"新增 / 修改 / 未改动 / 已知边界"四节；无内容就写"无"，不要留空。

---

## v1.0.0 — 2026-04-17

**Fork 基线：** 上游 superpowers `v5.0.7`（commit `b5576485`, 2026-04-16）

### 新增（Added）
- `THEME.md` —— 朝堂角色与工作流映射总纲；顶部"只做角色昵称皮肤"免责；昵称 → 真实 skill name → 真实目录三列对照表。
- `MAINTENANCE.md` —— 从上游拉取更新的 merge 操作手册（含备选 rebase / cherry-pick）；1566 层改动文件清单；自检 grep 与脚本语法自检命令。
- `commands/西苑问对.md`、`commands/拟票.md`、`commands/批红.md` —— 三条中文 slash command，本质是引导 agent 去调真实上游 skill。
- 14 处 `skills/*/SKILL.md` 顶部新增"【1566·xxx】"角色前引 blockquote（不动 skill 主体）。
- `agents/code-reviewer.md` 顶部加"【1566·都察院左都御史】"角色前引。

### 修改（Changed）
- `package.json`、`.claude-plugin/plugin.json`、`.claude-plugin/marketplace.json`、`.cursor-plugin/plugin.json`、`gemini-extension.json`：plugin 名改为 `superpowers1566`，version 重置为 `1.0.0`，描述改为 1566 主题。
- `README.md`、`AGENTS.md`、`CLAUDE.md`：按 1566 朝堂口吻重写；但硬规则（PR 94% 拒收率、Red Flags、contributor 要求等）一字不改。
- `GEMINI.md`：path 保持 `skills/using-superpowers/`（未改名）。
- `commands/brainstorm.md`、`commands/write-plan.md`、`commands/execute-plan.md`：从 "deprecated" 提示改为 "legacy alias" 提示，导向真实 skill name。
- `hooks/session-start`、`.opencode/plugins/superpowers.js`：banner 字样改为 `You have superpowers (大明王朝1566 版 · superpowers1566)`；同时把 banner 里指向 skill 的字面量从 `'superpowers:using-superpowers'` 改为 `'using-superpowers'`，让措辞与 plugin 名解耦，不再把过期的 plugin 前缀注入到 agent 的 session 上下文里。

### 未改动（Deliberately unchanged）
- 所有 skill 主体内容（Red Flags 表、合理化清单、HARD-GATE、RED-GREEN-REFACTOR 等）。
- Skill 目录名、`name:` frontmatter、跨 skill 内部引用中的 `superpowers:<skill-name>` 标识——按"只做皮肤、不动行为"的决策保留。（例外：`hooks/session-start` 与 `.opencode/plugins/superpowers.js` banner 文本里面向 agent 的 `'superpowers:using-superpowers'` 字面量已改为 `'using-superpowers'`——那不是跨 skill 引用，是 banner 的可读文本，去掉前缀避免把过期 plugin 名注入 session 上下文。详见上方"修改"。）
- `RELEASE-NOTES.md`、`docs/`、`scripts/`、`tests/`——上游 vendored，留给上游同步。
- `hooks/` 中除 `session-start` 的 banner 之外，其它文件（`hooks.json`、`hooks-cursor.json`、`run-hook.cmd`）未动，属 vendored。

### 已知边界
- 若上游发版，跨 skill 引用中的 `superpowers:<skill-name>` 字面量在本分叉仓库里仍然写 `superpowers:`；实际运行时，Claude Code 会按实际安装的 plugin（`superpowers1566`）解析 skill，Skill 工具的行为不受影响——但**如果你同时安装了原版 `superpowers` 插件与本分叉，会有命名冲突**。只装一个。
- 本分叉是面向学习与玩票的 themed fork，不对 upstream 开 PR（见 `AGENTS.md` "关于本分叉"一节）。
