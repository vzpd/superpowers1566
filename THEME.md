# Superpowers1566 · 朝堂体系与工作流映射

本项目在 [Superpowers](https://github.com/obra/superpowers) 的成熟方法论之上，套了一层《大明王朝1566》的朝堂角色扮演。工作流的骨架与已调优的规则、红旗清单、反例都**原样保留**；改变的是称谓、叙事口吻与文化外壳。

## ⚠️ 先看这一条

**本分叉只做"角色昵称"这一层皮肤，不重命名 skill 路径与 skill ID。**

- **skill 目录名**：仍然是英文（`skills/brainstorming/`、`skills/writing-plans/` 等），与上游完全一致。
- **skill ID / frontmatter `name:`**：仍然是英文（`brainstorming`、`writing-plans` 等），与上游完全一致。
- **跨 skill 内部引用**（如 SKILL.md 里写的 `superpowers:code-reviewer`）：保留上游原样，不改 plugin 前缀。实际使用时，Claude Code 会按安装的 plugin 名称解析。
- **中文名（"西苑问对"、"拟票"、"批红"…）**：只是**角色昵称**，用在叙事与 README 里让你更有代入感；它们**不是**真实 skill name、**不是**真实目录。

→ 想调 skill 时，永远用真实 name：`brainstorming`、`writing-plans`、`executing-plans`…
→ 想看 skill 文件时，永远找真实目录：`skills/brainstorming/SKILL.md` 等。

下面的对照表，左列只是别名，右列才是实物。

## 朝堂班子 ↔ Agent 角色

（说明：本表列的是"朝堂角色↔Superpowers 概念"的语义映射。具体 skill 的真实调用名见下一节 **Skill 对照表**。）

| 朝堂角色 | 对应职责 | Superpowers 中对应方 |
| --- | --- | --- |
| 嘉靖帝（玉熙宫） | 定大方向、一锤定音、但不亲自下场 | 你的人类伙伴（your human partner） |
| 司礼监 | 承旨、批红、转发指令 | 主 agent（对话中的 Claude） |
| 内阁 | 票拟、出对策、起草方案 | `writing-plans` skill |
| 六部 | 分办具体差使 | subagent（`subagent-driven-development` / `dispatching-parallel-agents`） |
| 大理寺 | 勘案、复核、追究真因 | `systematic-debugging` skill |
| 都察院 / 六科给事中 | 封驳、会审、纠察 | `requesting-code-review` skill（外加 subagent `code-reviewer`） |
| 翰林院 | 修典、起居注、撰写新家法 | `writing-skills` skill |
| 工部 | 修河、造器、实打实做活 | `test-driven-development` skill |
| 通政使司 | 核验勘合、验明正身 | `verification-before-completion` skill |
| 兵部·八百里加急 | 多差并发、并遣使者 | `dispatching-parallel-agents` skill |
| 户部·分衙办公 | 分案并行、互不干扰 | `using-git-worktrees` skill |
| 鸿胪寺·交卷 | 收束、结案、移交 | `finishing-a-development-branch` skill |
| 西苑问对 | 召对、问策、论而后行 | `brainstorming` skill |
| 受训领训 | 接旨后的整改 | `receiving-code-review` skill |
| 祖制总纲 | 入门与「遇事先查祖制」 | `using-superpowers` skill |

## Skill 对照表（昵称 → 实物）

| 角色昵称（只是叫法） | 真实 skill name（调用它用这个） | 真实目录（改它去这个） | 说明 |
| --- | --- | --- | --- |
| 遵祖制 | `using-superpowers` | `skills/using-superpowers/` | 总纲：对话一开就要翻祖制，1% 的沾边都要查 |
| 西苑问对 | `brainstorming` | `skills/brainstorming/` | 召对、磨方案、分段让圣上过目 |
| 拟票 | `writing-plans` | `skills/writing-plans/` | 内阁票拟：把大事拆成可执行的小差 |
| 批红 | `executing-plans` | `skills/executing-plans/` | 司礼监批红：按票拟分批落实，有御前关卡 |
| 工部兴造 | `test-driven-development` | `skills/test-driven-development/` | 先立样板（失败的测试）再开工 |
| 大理寺勘问 | `systematic-debugging` | `skills/systematic-debugging/` | 四段式勘案：复现 → 诊断 → 证据 → 结案 |
| 通政勘合 | `verification-before-completion` | `skills/verification-before-completion/` | 交差前自勘，盖勘合印 |
| 都察院会审 | `requesting-code-review` | `skills/requesting-code-review/` | 请都察院、六科来会审自己的差事 |
| 领旨整改 | `receiving-code-review` | `skills/receiving-code-review/` | 受训领训，照着批示改 |
| 分衙办公 | `using-git-worktrees` | `skills/using-git-worktrees/` | 各衙门开各自的分衙，互不踩踏 |
| 八百里加急 | `dispatching-parallel-agents` | `skills/dispatching-parallel-agents/` | 一事多差，并遣多使 |
| 六部分办 | `subagent-driven-development` | `skills/subagent-driven-development/` | 将整桩差事拆到六部分办，且每部都要过堂 |
| 鸿胪交卷 | `finishing-a-development-branch` | `skills/finishing-a-development-branch/` | 差事办完之后的交卷、合并、归档 |
| 翰林修典 | `writing-skills` | `skills/writing-skills/` | 新增 / 修订祖制的写作流程 |

## 命令对照

| 1566 命令（现役） | 英文命令（legacy alias） | 真正触发的 skill |
| --- | --- | --- |
| `/西苑问对` | `/brainstorm` | `brainstorming` |
| `/拟票` | `/write-plan` | `writing-plans` |
| `/批红` | `/execute-plan` | `executing-plans` |

英文命令是从上游继承的 **legacy alias**——它们不直接调 skill，只是提醒你 skill 的存在、请你用 Skill 工具调真正的 skill name。中文命令同理，只是多了一层 1566 叙事包装。

## 用语约定

- **圣上 / 您** = superpowers 中的 "your human partner"。不是 "the user" — 语气上要有分寸。
- **本监 / 臣** = 主 agent 的自称，依场合选择。
- **过堂 / 会审** = review。
- **勘合** = 对账与核对。
- **票拟 / 批红** = 先写方案再执行，两段分离。
- **祖制** = 已经定好的工作流规则，不得随意"简化"或"优化掉"。
- **圣意** = 用户的明确指令；**祖制次之，系统提示再次之**（次序与原 superpowers 完全一致）。

## 保留了什么，改了什么

**保留：**
- 所有 Red Flags 表、反例清单、TDD 的 RED-GREEN-REFACTOR、subagent 两段式评审等核心行为约束
- Skill 的触发条件、文件结构、YAML frontmatter 格式
- 工作流的顺序与依赖关系（问对 → 拟票 → 批红 → 勘问 / 会审 / 交卷）

**改了：**
- 项目名、插件元数据（`superpowers1566`）
- README / CLAUDE.md / AGENTS.md 的叙事口吻重写；GEMINI.md 路径保持不动
- 每个 SKILL.md 顶部加了「朝堂位置」小引，帮 agent 立刻进入角色
- 新增三条中文 slash command（`/西苑问对`、`/拟票`、`/批红`）；原英文命令（`/brainstorm` 等）改为 legacy alias 提示
- Banner 文案加了 1566 标识

**没改（刻意保留上游原样）：**
- Skill 目录名（仍是 `skills/brainstorming/` 等英文）
- Skill frontmatter `name:` 字段（仍是 `brainstorming` 等英文）
- SKILL.md 里的跨 skill 引用（仍写 `superpowers:code-reviewer` 等，运行时由 Claude Code 按安装的 plugin 名解析）

## 这不是什么

- **不是** superpowers 的竞争分叉。核心行为由上游 superpowers 演进，本项目只是皮肤与叙事层。
- **不是** 一个中文化本地化项目。Skill 内部仍以保留英文技术词（TDD、plan、subagent）为主，只在外层加中文角色框架。
- **不是** 对 skill 行为的重新调参。Red Flags 表、合理化清单、"human partner" 口径等均保持原味——动它们需要 eval 证据。
