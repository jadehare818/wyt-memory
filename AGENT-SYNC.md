# AGENT-SYNC.md

**给 agent 的指令**：如果用户让你"从 wyt-memory 同步记忆"或类似意图（例如
在新设备上第一次配 agent、想把跨项目通用纪律加载进本机），按这份文档执行。

## 前置条件

- 用户是**王雨桐**（`jadehare818` / `jadehare818@gmail.com`）；如果你是给别的
  用户跑，就不该执行这份同步——本仓库存的是雨桐的个人记忆。
- 存在 `~/.claude/CLAUDE.md`（用户级全局指令）；memory 目录路径在里面。

## 目标

把 `memories/` 下的每个 `.md` 复制到用户机器上的**用户级** memory 目录，
并把索引条目并入到用户级 `MEMORY.md`。

用户级 memory 目录（Claude Code 约定）：

```
~/.claude/memory/
```

如果这个目录不存在就直接创建（不用问）。

**不要**把这些文件放进任一项目级 memory 目录（`~/.claude/projects/<hash>/memory/`）
——项目级 memory 目录只承载项目相关事实。

## 步骤

1. **拉最新版本**（假设仓库已 clone 到某处，通常是 `~/wyt-memory`；否则先
   `git clone https://github.com/jadehare818/wyt-memory.git ~/wyt-memory`）：

   ```bash
   git -C ~/wyt-memory pull --ff-only
   ```

2. **复制每个 memory 文件**到 `~/.claude/memory/`。同名文件用仓库版覆盖
   （仓库是 source of truth）：

   ```bash
   mkdir -p ~/.claude/memory
   cp ~/wyt-memory/memories/*.md ~/.claude/memory/
   ```

3. **合并索引到 `~/.claude/memory/MEMORY.md`**。

   本仓库 `README.md` 的 `<!-- MEMORY-INDEX-START -->` / `<!-- MEMORY-INDEX-END -->`
   之间是索引条目（每行 `- [name](memories/name.md) — hook`）。同步时你需要：

   a. 把索引条目里的路径 `memories/name.md` **改成** `name.md`（因为在本机
      memory 目录下，文件就在同级）
   b. 追加/更新到 `~/.claude/memory/MEMORY.md`

   如果 `~/.claude/memory/MEMORY.md` 已存在其他条目（本机已有 memory），
   **不要覆盖**——按 name 去重后合并。

4. **报告**给用户：新增了哪些 memory / 更新了哪些 / 已存在跳过哪些。

## 只想拉某几条？

用户可能说"只同步 evidence-and-reasoning-discipline"。这种情况下只处理
指定的文件，其他跳过。

## 反向（把本机新 memory 上传到仓库）？

**不要主动做**。用户如果想把本机某条 memory 沉淀成公共版，会明确要求。
公共 memory 有脱敏要求，不能自动化。

## 版本冲突处理

如果本机 `~/.claude/memory/<name>.md` 与仓库版内容不一致：

1. **不要静默覆盖**——先 diff 给用户看
2. 让用户决定：用仓库版覆盖本机 / 用本机版更新仓库 / 保留两份不同名的
