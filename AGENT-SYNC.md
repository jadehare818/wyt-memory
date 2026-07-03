# AGENT-SYNC.md

**给 agent 的指令**：如果用户让你"从 wyt-memory 同步记忆 / 装某条 memory /
把这个仓库里的记忆加到本机"或类似意图，按这份文档执行。

## 前置事实（不要臆测）

- 用户是**王雨桐**（`jadehare818`）；如果不是雨桐本人使用，先跟用户确认再执行。
- Claude Code **没有"用户级 memory 目录"**这回事。所有 `.md` memory 文件都
  存放在 `~/.claude/projects/<hashed-cwd>/memory/` 下，只在对应 cwd 起 session
  时才会自动加载。
- 目录命名规则：**cwd 里的 `/` 替换成 `-`，其它字符原样保留**。
  例如 `/Users/bytedance/maas-backend` → `-Users-bytedance-maas-backend`。
  从目录名**反推 cwd 存在歧义**（`maas-backend` 也可能对应 `maas/backend`），
  所以列给用户看时**不要猜**，把带连字符的目录名原样呈现，让用户自己识别。
- 同一台机器上会有多个项目 memory 目录，每个都独立。**不要跨项目复制**——
  用户会明确告诉你装到哪个项目。
- **不存在"装到全局"**这个选项。若用户想让某条 memory 每个 session 都加载，
  唯一路径是让 `~/.claude/CLAUDE.md` `@`-include 它——但这不属于本 sync 流程，
  用户会单独提。

## 交互流程

### Step 1 — 列仓库里可用的 memory

对 `memories/` 下每个 `.md`，读出它的 frontmatter，列给用户：

```
1. evidence-and-reasoning-discipline
   排障 / 读代码 / 解读日志时都要遵守的纪律：证据不足给猜测 · 名字≠逻辑 · "没看到 X"≠"X 不存在" · 日志对不上先对齐时间

2. <next memory name>
   <its description from frontmatter>
```

不要总结、不要合并、不要主观判断哪个"更值得装"——用户自己决定。

### Step 2 — 列本机现有的项目 memory 目录

扫 `~/.claude/projects/*/memory/`，列给用户看**目录名**（不要猜真实 cwd）：

```
本机现有项目 memory 目录：

  A. -Users-bytedance-maas-backend        (10 条 memory)
  B. -Users-bytedance                     (9 条 memory)
  C. -Users-bytedance-yopo-agent          (1 条 memory)
```

如果 `~/.claude/projects/` 下没有 memory 目录，直接告诉用户"还没有项目
memory 目录，请先在某个项目里让 Claude Code 建过至少一条 memory 再试"，
不要凭空创建目录。

### Step 3 — 逐条问用户

对 Step 1 列出的每条 memory，问一次：

- 装到 A（`-Users-bytedance-maas-backend`）？
- 装到 B？
- 装到 C？
- 不装？

允许多选（同一条 memory 可以同时进多个项目）。

### Step 4 — 执行安装

对每个"要装"的 (memory, target-dir) 对：

1. 目标路径 = `~/.claude/projects/<dir-name>/memory/<memory-name>.md`
2. **同名文件冲突处理**：如果目标已存在，先 `diff` 给用户看，让用户选：
   - 用仓库版覆盖本机版
   - 保留本机版（跳过）
   - 重命名（用户提供新 name）
   
   **不要静默覆盖**。
3. 复制文件到目标目录。
4. 更新目标目录的 `MEMORY.md`——按 name 去重后，加入一行：
   `- [<name>](<name>.md) — <description>`
   
   如果 `MEMORY.md` 不存在，创建它。

### Step 5 — 汇总回报

告诉用户：

- 装到 A 的：xxx, yyy
- 装到 B 的：zzz
- 跳过的：...
- 冲突处理结果：...

## 只想装某一条？

用户可能说"只装 evidence-and-reasoning-discipline"。那就只走 Step 1
把这一条列出来（可以省略），Step 2 & 3 & 4 & 5 照走。

## 反向（把本机新 memory 上传到仓库）？

**不要主动做**。用户明确要求时才做，且需先脱敏——公共 memory 不能自动化。

## 更新已装的 memory

用户可能说"仓库里 xxx 更新了，同步到 A"——按 Step 4 的冲突处理走 diff 流程。
