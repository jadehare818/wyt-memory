# wyt-memory

雨桐的个人 Claude Code memory 仓——跨项目通用的思维纪律、排障笔记与工作原则。

存的都是"跨项目通用、值得每个 session 都加载一次"的东西：
判断纪律、读代码习惯、排障 SOP。**不放**项目相关事实（那类内容留在
每个项目自己的 `~/.claude/projects/<hash>/memory/` 里）。

## Index

<!-- MEMORY-INDEX-START -->
- [evidence-and-reasoning-discipline](memories/evidence-and-reasoning-discipline.md) — 排障 / 读代码 / 解读日志时都要遵守的纪律：证据不足给猜测 · 名字≠逻辑 · "没看到 X"≠"X 不存在" · 日志对不上先对齐时间
<!-- MEMORY-INDEX-END -->

## 用途

在任何新设备上配 Claude Code / 其他 agent 时，把这个仓库拉下来，
让 agent 读 `AGENT-SYNC.md`，它会自动把相关 memory 加载进本机
memory 目录并更新 `MEMORY.md` 索引。详见 [`AGENT-SYNC.md`](AGENT-SYNC.md)。

## 结构

```
wyt-memory/
├── README.md              # 你正在看的这份索引
├── AGENT-SYNC.md          # 给 agent 的同步指令（agent 读这份）
└── memories/              # 每个 memory 一个 .md，带 frontmatter
    └── evidence-and-reasoning-discipline.md
```

## 添加新 memory

1. 在本地某个项目里让 memory 稳定运行一段时间，确认它是**跨项目**通用的
2. 复制到 `memories/` 下，保留原 frontmatter（`name`, `description`, `metadata.type`）
3. 在本 README 的 `<!-- MEMORY-INDEX-* -->` 之间加一行索引条目：
   `- [name](memories/name.md) — 一句 hook`
4. commit + push

## Frontmatter 约定

每个 memory 文件必须以 YAML frontmatter 开头，字段：

```yaml
---
name: <kebab-case-slug>            # 与文件名（不含扩展名）一致
description: <一句话摘要>          # 用于 agent recall 时判断相关性
metadata:
  type: user | feedback | project | reference
---
```

- `user` — 关于用户身份/角色/偏好的事实
- `feedback` — 用户对 agent 工作方式的反馈（含"为什么"）
- `reference` — 指向外部资源（URL / dashboard / 文档）
- `project` — **不应出现在这个仓库**（放各项目自己的 memory 里）
