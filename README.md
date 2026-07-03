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
让 agent 读 [`AGENT-SYNC.md`](AGENT-SYNC.md)。它会：

1. 列出仓库里每条 memory（name + description）
2. 列出本机现有的项目 memory 目录
3. **逐条问你**这条 memory 装到哪个项目 / 不装（不做任何默认预选）
4. 遇到同名文件时贴 diff 让你选覆盖/保留/重命名
5. 更新目标项目的 `MEMORY.md` 索引

Claude Code 目前**没有"跨项目全局 memory 目录"**这个概念——所有
memory 都是绑在项目上的。如果想让某条 memory 每个 session 都加载，
另外一条路是让 `~/.claude/CLAUDE.md` `@`-include 它（这不在 sync 流程里，
用户按需自己加）。

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
- `project` — 项目相关的事实

**关于 `project` 类型**：这类 memory 通常只对某个项目有意义、放在这个跨项目
仓库里意义不大。除非你确定多个项目会共用（比如公司级别的服务架构描述），
否则建议留在本地项目 memory 目录，不要推到这里。
