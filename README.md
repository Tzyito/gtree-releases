# gtree — workspace for AI coding agents

**Website: [gtree.dobbylab.com](https://gtree.dobbylab.com/)** · [日本語](https://gtree.dobbylab.com/ja/) · [中文](https://gtree.dobbylab.com/zh/) · [Blog](https://gtree.dobbylab.com/blog/) · [Release notes](https://gtree.dobbylab.com/release-notes/) · [X @dobbylabhq](https://x.com/dobbylabhq) · [Discord](https://discord.gg/EnPJxrUEK5)

gtree is a native desktop app for people who run several AI coding agents at once (works with Claude Code and Codex), built on git worktrees:

- **Agents see each other.** Agents in the same worktree agree who edits first; overlaps across worktrees are flagged before they collide.
- **Every turn can be rolled back**, and new sessions remember the last one.
- **One inbox for every project** — see which agent is working and which one needs you.
- **A real Git client underneath** — worktree switching, Diff between any two branches or commits.

Local features are free forever, no sign-up. gtree never uploads your code.

This repository hosts the installers and the auto-update feed. The source code is not here.

## Install

Currently available for macOS (Apple Silicon):

```bash
curl -fsSL https://gtree.dobbylab.com/install.sh | bash
```

It installs into `~/Applications/gtree.app` and opens on its own. When a new version is out, gtree shows “Update and restart” at the bottom of the sidebar.

Please use the command above rather than downloading the zip in a browser — browser downloads get stopped by macOS Gatekeeper the first time.

---

## 中文

gtree 是给同时跑多个 AI coding agent 的人用的原生桌面应用(支持 Claude Code 和 Codex):agent 之间互相看得见、撞车时自己商量;每一轮都能反悔;一个收件箱管所有项目。官网:<https://gtree.dobbylab.com/zh/>

本仓库只放安装包与自动更新源,源码不在这里。安装(macOS Apple Silicon):

```bash
curl -fsSL https://gtree.dobbylab.com/install.sh | bash
```

请用上面的命令安装,不要从浏览器下载 zip —— 浏览器下载的 app 第一次会被 macOS 拦下。
