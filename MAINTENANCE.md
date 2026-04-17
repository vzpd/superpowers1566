# 维护手册：从上游 superpowers 拉取更新

本文件写给本分叉的维护者（人或 AI）。目的：**当上游 [obra/superpowers](https://github.com/obra/superpowers) 发新版时，把 skill 主体的变更拉回来，同时保住本分叉的 1566 换皮层。**

---

## 核心原则

**1566 分叉只加一层薄皮，不改 skill 行为。**
所以维护策略是：**以上游为主干，把 1566 改动作为可识别、可重放的一小组 diff 来维护。**

---

## 1566 层涉及的文件清单（Last updated 2026-04-17）

所有在本分叉里**实际做过主动修改**的文件。上游同步时，它们是"冲突可能点"；其它文件如有冲突，几乎一定是上游变动，直接采用上游版本。

> 这是**按文件索引**的清单（给同步时逐文件解决冲突用）。
> 按版本叙述的"改了什么、为什么改"见 [`CHANGELOG-1566.md`](./CHANGELOG-1566.md)。
> 两份文档应保持一致——本清单若有新增/移除，请同步更新 changelog。

### 完全新增（上游没有）
- `THEME.md`
- `MAINTENANCE.md`（本文件）
- `CHANGELOG-1566.md`
- `commands/西苑问对.md`
- `commands/拟票.md`
- `commands/批红.md`

### 元数据类（基本每次都要改）
- `package.json`
- `.claude-plugin/plugin.json`
- `.claude-plugin/marketplace.json`
- `.cursor-plugin/plugin.json`
- `gemini-extension.json`

### 顶层文档（1566 口吻重写）
- `README.md`
- `AGENTS.md`
- `CLAUDE.md`
- `GEMINI.md`

### Skill 前引（在每个 SKILL.md 顶部、frontmatter 之后插一段 blockquote）
- `skills/using-superpowers/SKILL.md`
- `skills/brainstorming/SKILL.md`
- `skills/writing-plans/SKILL.md`
- `skills/executing-plans/SKILL.md`
- `skills/test-driven-development/SKILL.md`
- `skills/systematic-debugging/SKILL.md`
- `skills/verification-before-completion/SKILL.md`
- `skills/requesting-code-review/SKILL.md`
- `skills/receiving-code-review/SKILL.md`
- `skills/using-git-worktrees/SKILL.md`
- `skills/finishing-a-development-branch/SKILL.md`
- `skills/dispatching-parallel-agents/SKILL.md`
- `skills/subagent-driven-development/SKILL.md`
- `skills/writing-skills/SKILL.md`

### Agent 前引
- `agents/code-reviewer.md`

### 命令（legacy alias 提示）
- `commands/brainstorm.md`
- `commands/write-plan.md`
- `commands/execute-plan.md`

### Banner 品牌词
- `hooks/session-start`（一处 banner 文本）
- `.opencode/plugins/superpowers.js`（一处 banner 文本）

---

## 同步流程（merge 法，推荐）

适用场景：上游发新版，想把所有 skill 主体更新拉过来。

**为什么推荐 `merge`：** 本分叉的 1566 改动分散在顶层文档、14 份 SKILL.md 前引、若干 command 和 banner 中。用 `rebase upstream/main` 会让每个含 1566 改动的 commit 都在 rebase 时重新触发冲突；用 `merge upstream/main` 只触发一次合并冲突，逐文件解决即可。

**前置：** 本仓库已有 `origin`（你自己的远端），还需要一个指向上游的 `upstream`：

```bash
# 若已存在，跳过（精确匹配远端名，不会被 upstream2 之类的前缀名误导）
git remote get-url upstream >/dev/null 2>&1 || git remote add upstream https://github.com/obra/superpowers.git
git fetch upstream
```

**步骤：**

```bash
# 0. 了解本地 main 的状态（不要盲目 push；1566 主线的未推送 commit 可能还要 review）
git status
git log @{u}..HEAD --oneline    # 看看本地有没有未推送

# 1. 拉上游最新
git fetch upstream

# 2. 开一个新分支做合并，留退路
git checkout -b sync-upstream-$(date -u +%Y%m%dT%H%M%SZ) main

# 3. 合入上游（主路径）
git merge upstream/main

# 4. 解决冲突。按上面"文件清单"逐个扫：
#    - 顶层文档 (README/AGENTS/CLAUDE/GEMINI/THEME) —— 保留 1566 口吻，但要把上游新增的 skill 对应的条目加进工作流表与 THEME 对照
#    - 元数据 —— 保持 name = superpowers1566；version 看是否需要 bump；其它字段跟上游
#    - 每个 SKILL.md —— 保留顶部 1566 blockquote，body 采纳上游
#    - commands/brainstorm.md 等 legacy alias —— 保留 1566 描述
#    - hooks/session-start、.opencode/plugins/superpowers.js —— 保留 banner 1566 字样

# 5. 如果上游新增了 skill：
#    - 在 THEME.md 对照表里加一行（起一个 1566 角色昵称 + 真实 name + 真实目录）
#    - 在新 skill 的 SKILL.md 顶部加一段 blockquote 前引
#    - 如果该 skill 有对应的 slash command，也考虑加一条 1566 中文 command

# 6. 如果上游删除了 skill：删对应的 1566 前引、删对照表条目、删对应 command（如果有）

# 7. 验证：Grep 检查没有虚构路径、虚构 skill id 残留（见下方自检 grep）；
#    对改动到的脚本做语法检查：
#      bash -n hooks/session-start
#      node --check .opencode/plugins/superpowers.js
```

**自检 grep（同步完跑一下）：**

```bash
# 1. 不应该有虚构中文目录路径（--glob '!MAINTENANCE.md' 排除本文件里的枚举示例）
rg 'skills/(遵祖制|西苑问对|拟票|批红|工部兴造|大理寺勘问|通政勘合|都察院会审|领旨整改|分衙办公|八百里加急|六部分办|鸿胪交卷|翰林修典)' . --glob '!MAINTENANCE.md'

# 2. 不应该有虚构 plugin-prefixed skill id（排除本文件本身——它里含这个字面量作为检查目标）
rg 'superpowers1566:' . --glob '!MAINTENANCE.md' --glob '!CHANGELOG-1566.md'

# 3. GEMINI.md 应指向真实目录，预期返回两行：
#    @./skills/using-superpowers/SKILL.md
#    @./skills/using-superpowers/references/gemini-tools.md
rg '@\./skills/' GEMINI.md

# 4. 不应该在本分叉活跃路径里有未换皮的旧 banner（仅 docs/plans/ 下的 vendored 历史允许残留）
rg 'You have superpowers\.' . --glob '!docs/**' --glob '!RELEASE-NOTES.md'
```

预期：①②④ 返回 0 条；③ 返回两行，都指向 `skills/using-superpowers/`。

---

## 同步流程（rebase / cherry-pick 法，仅在历史极简时用）

`rebase upstream/main` 看似历史干净，但会让**每一个**含 1566 改动的 commit 都在 rebase 过程中单独触发冲突——对本分叉这种零散改动并不友好。仅建议在"本分叉只有一个 squashed 的 1566 commit"时考虑：

1. 把 1566 的一组改动整理成一个 squashed commit `1566-layer`。
2. 每次同步时：`git reset --hard upstream/main` → `git cherry-pick 1566-layer`。
3. 冲突只会发生在 1566 涉及文件。

代价：commit 历史线性，但 1566 的 incremental 改动会被 squash 掉，PR/回溯都不方便。

---

## 明确不支持的操作

- **不要向 upstream 开 PR。** `AGENTS.md` 的 "关于本分叉" 一节已说明：行为改动走 upstream，rebrand 改动不合 upstream 口。本分叉的 PR 必定被封驳。
- **不要在 1566 层修改 skill 主体行为。** Red Flags、HARD-GATE、RED-GREEN-REFACTOR 等全是上游调优过的；要动它们，回 upstream 出 eval 证据。
- **不要重命名 skill 目录或 skill `name:` 字段。** 这会让本分叉的 skill 引用无法与上游对齐，同步代价指数级升高。
- **不要跑 `scripts/sync-to-codex-plugin.sh`。** 这是上游用于把 superpowers 同步到 `prime-radiant-inc/openai-codex-plugins` 仓库的内部脚本，对本分叉完全无意义——跑它会尝试去推一个你没有权限的仓库，并把本仓库当成"上游 superpowers"来处理。本分叉维护不需要这个脚本。

## 可以跑的脚本

本仓库目前只有**一条**维护脚本可直接用。`scripts/` 下其它文件（如 `sync-to-codex-plugin.sh`）都是上游内部脚本——见上方"明确不支持的操作"。

- **`scripts/bump-version.sh <new-version>`** — 版本号统一升级。
  - 已在 `.version-bump.json` 的 `audit.exclude` 中追加 `CHANGELOG-1566.md` 与 `MAINTENANCE.md`；将来升级到新版本（例如 2.0.0）时，`CHANGELOG-1566.md` 里历史条目保留的旧版本号（含 fork 基线 `v5.0.7` 等）不会被 audit 当作残留误报。
  - `--audit` 模式**仍可能在 `tests/` 与 `docs/plans/` 下报几条"UNDECLARED files containing '<version>'"**——这些是上游 vendored 文件（测试 fixture、示例 JSON）里与版本号巧合的字面量，不是本分叉要升级的目标，**无视即可**。不需要加进 exclude，因为 exclude 过度扩张会让真正的遗漏不被发现。

---

## 如果你是 AI 维护者

看到"同步一下上游"的指示时：
1. 读完 `CHANGELOG-1566.md` 最新一条，知道上次基线是哪个 commit。
2. 按上面 **同步流程（merge 法，推荐）** 走；只在本分叉历史被 squash 成单个 commit 时才考虑 rebase / cherry-pick 备选路径。
3. 跑自检 grep（文档里的那四条）和脚本语法自检（`bash -n hooks/session-start`、`node --check .opencode/plugins/superpowers.js`）。
4. 在 `CHANGELOG-1566.md` **顶部**新增一条记录（最新的在上），格式参照已有条目：写清楚"本次同步自上游 commit `<sha>`、新基线版本号、冲突在哪些文件、如何解决"。
5. 不自作主张地重命名目录、不"顺手优化" skill 主体、不向 upstream 开 PR。
