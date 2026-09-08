# Personal Skills

个人维护的自定义 Agent Skills，按需精选和调整。

## 安装

需要 Git、Node.js/npm，以及目标 Agent。新电脑安装到 Codex：

```bash
npx --yes skills@latest add yearth/skills -g -a codex --skill brainstorming minimal-change task-handoff -y
```

安装到多个 Agent：

```bash
npx --yes skills@latest add yearth/skills -g -a codex claude-code pi --skill brainstorming minimal-change task-handoff -y
```

先查看可安装内容：

```bash
npx --yes skills@latest add yearth/skills --list
```

上述安装命令跳过确认；已有同名 Skill 时，请先保留本地定制版本。安装器支持的 Agent 和行为见 [Skills CLI](https://github.com/vercel-labs/skills)。

## 更新

```bash
npx --yes skills@latest update brainstorming minimal-change task-handoff -g -y
```

更新从本仓库获取版本，不会把本机修改自动上传到 GitHub。希望跨电脑复用的调整，应先提交到本仓库，再在其他电脑更新。

## 收录

| Skill | 用途 |
| --- | --- |
| [brainstorming](skills/brainstorming/SKILL.md) | 澄清模糊需求、比较设计方案；按不确定性控制讨论深度，保留浏览器视觉伴侣。 |
| [minimal-change](skills/minimal-change/SKILL.md) | 完整解决任务，控制行为影响范围，避免无关扩张和为缩小 diff 而漏修。 |
| [task-handoff](skills/task-handoff/SKILL.md) | 选择最轻的续作方式；有原生任务工具时直接交接，否则提供可复制的中文启动说明。 |

brainstorming 不强制 spec、自动 commit 或重复审批。视觉伴侣包含完整指南、服务脚本及页面资源，需要 Node.js 和 Bash；Windows 请使用 Git Bash 或 WSL。首次使用视觉伴侣时由 Agent 按指南启动。本仓库发布不代表对所有操作系统、Agent 或视觉效果都已验证。

## 来源与许可

brainstorming 基于 [obra/superpowers](https://github.com/obra/superpowers) 的同名 Skill，经本地定制；入口已精简，视觉伴侣资源保留本地已有版本（含调整），不声称与上游当前版本完全相同。已移除不再使用的 spec reviewer 提示词。

采用 [MIT License](LICENSE)，保留上游版权声明。仓库只收录选定 Skill，不包含本机账号、凭证或全局 Agent 配置。
