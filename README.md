# Superpowers · 大明王朝1566 版

> 万事按祖制行之。——本项目把 [Superpowers](https://github.com/obra/superpowers) 的整套软件开发方法论，照搬进嘉靖朝堂：票拟、批红、会审、勘合，一样都不能少。

## 什么是 Superpowers1566

**Superpowers 的骨架（TDD / 调试 / 协作的 skill 库与 hooks），原封不动。**
**外壳换成大明王朝1566 的朝堂体系。**

你与 agent 的关系从「user → assistant」变成「圣上 → 司礼监」：
- 圣上（您）裁定大方向；
- 司礼监（主 agent）承旨、批红、统筹六部；
- 内阁起草方案（对应 `writing-plans` skill），分段呈览；
- 六部 / subagent 分办具体差事（对应 `subagent-driven-development`），每件差事都要过堂；
- 大理寺负责调试追因（对应 `systematic-debugging`）；
- 都察院负责 code review 与封驳（对应 `requesting-code-review`）；
- 工部守的是 TDD（对应 `test-driven-development`）：先立样板（失败测试）再开工。

> 说明：中文名是**角色昵称**；真实 skill name 与目录仍是英文，与上游一致。完整对照见 [THEME.md](./THEME.md)。

## 核心理念（一字未改）

- **先奏后行（TDD）**：先写测试，并目睹其失败。
- **凡事查祖制（Skills First）**：对话一开，1% 可能沾边的 skill 都要先查。
- **圣意 > 祖制 > 系统提示**：CLAUDE.md / AGENTS.md / GEMINI.md 里的圣意，永远最大。
- **一事一奏（One problem per PR）**：不搭车、不打包。
- **勘合核验（Evidence over claims）**：没有勘合不算办完。

## 工作流

格式：**"角色昵称" → 真实 skill name**。

1. **西苑问对 → `brainstorming`** — 圣上召对，把"我想做 X"磨成一份可读的设计。分段呈览，读得下去的长度。
2. **拟票 → `writing-plans`** — 内阁照着设计写票拟：把每件差事拆到"2-5 分钟一件"的尺寸，路径、代码、验证步骤写全。
3. **批红 → `executing-plans`（或 `subagent-driven-development` 二选一）** — 司礼监分派六部办理。平台支持 subagent 时优先走 `subagent-driven-development`，否则走 `executing-plans`。每件差经两道过堂：先对着票拟核、再看代码质量。
4. **工部兴造 → `test-driven-development`** — 先写会红的测试，红；写最小代码，绿；提交；再重构。跳过红的这一步 → 返工。
5. **大理寺勘问 → `systematic-debugging`** — 出了 bug 别急着打补丁。四段式：复现、定位、证据、结案。
6. **都察院会审 → `requesting-code-review`** — 差办完请都察院来审，按严重等级处理；critical 问题封驳、不放行。
7. **鸿胪交卷 → `finishing-a-development-branch`** — 交卷、合并、归档；分衙（worktree）处理干净。

## 安装

> 说明：这是一份面向学习与玩票的 1566 themed 分叉。要生产用 superpowers，请使用上游官方插件。

**Claude Code（本地仓库开发模式）**

```bash
# 在 Claude Code 中注册本地 marketplace
/plugin marketplace add /path/to/superpowers1566
# 安装
/plugin install superpowers1566@superpowers1566-dev
```

**其他 harness** 的安装方式与上游 superpowers 一致，把插件名换成 `superpowers1566` 即可。详见 [THEME.md](./THEME.md) 与原仓库说明。

## 命令

| 1566 命令（现役） | 英文命令（legacy alias） | 触发的 skill |
| --- | --- | --- |
| `/西苑问对` | `/brainstorm` | `brainstorming` |
| `/拟票` | `/write-plan` | `writing-plans` |
| `/批红` | `/execute-plan` | `executing-plans` |

英文命令是从上游继承下来的 **legacy alias**——它们本身不执行 skill，只是提醒 agent 去调真实 skill。中文命令同理。

## 目录

- `skills/` — 十四份祖制（详见 [THEME.md](./THEME.md) 中的目录映射）
- `commands/` — slash 命令
- `agents/` — 专差 agent（如会审官 `code-reviewer`）
- `hooks/` — session start 等 hook
- `docs/` — 使用与移植文档
- `tests/` — 上游 superpowers 的 eval 与示例

## 许可

MIT，与上游 superpowers 一致。

## 致谢

- 方法论、skill 内容、hook 设计均来自 [Jesse Vincent](https://blog.fsck.com) 与 superpowers 团队。本项目只负责换皮。
- 朝堂角色取自电视剧《大明王朝1566》（张黎导演，刘和平编剧）。
