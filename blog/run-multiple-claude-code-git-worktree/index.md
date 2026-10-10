# How to Run Multiple Claude Code Sessions with Git Worktree

> Give each Claude Code session its own git worktree so parallel AIs never share a folder. The exact commands, the gotchas, and how to skip the busywork.

Published: 2026-10-10 · DobbyLab · https://gtree.dobbylab.com/blog/run-multiple-claude-code-git-worktree/

To run several Claude Code sessions on the same repository at once, give each one its own **git worktree**: a separate working folder, on its own branch, that shares the same `.git` history. Create one with `git worktree add ../myapp-fix-login -b fix-login`, start `claude` inside it, and repeat for every task. The AIs can then edit, build and test in parallel without overwriting each other's files.

That's the whole idea. The rest of this guide covers the commands you'll actually type, the snags people hit in the first week, and how gtree takes care of the repetitive parts.

![The gtree Workbench: the sidebar lists six worktrees of the nimbus-notes project, each with a status icon, and the middle shows one card per worktree under a 2 files overlap warning](/media/app/workbench-agents-light.webp "The gtree Workbench: one card per worktree, and the sidebar icons tell you which AI is working and which is waiting for you")

## Why not just open two Claude Code sessions in the same folder?

You can, and for read-only questions it's fine. But as soon as both sessions start editing, they share one working directory with no isolation at all:

- Session A rewrites `auth.ts`; session B, working from an older read of the file, changes it again.
- Session A's half-finished change breaks the build, and every test session B runs now fails for reasons it can't see.
- When you finally commit, the changes from both tasks are tangled together in one diff.

Git branches alone don't solve this, because a single folder can only have one branch checked out. Worktrees do: each one is a real directory with its own checked-out branch, its own uncommitted changes and its own build output, while all of them share the same commits, branches and remotes.

## How do I set up git worktrees for Claude Code?

From inside your main checkout:

```bash
# 1. Create a worktree next to the repo, on a new branch
git worktree add ../myapp-fix-login -b fix-login

# 2. Start Claude Code inside it
cd ../myapp-fix-login
claude
```

Open a second terminal and do the same for the next task:

```bash
git worktree add ../myapp-search -b search-box
cd ../myapp-search
claude
```

A few commands you'll use constantly:

| Command | What it does |
|---|---|
| `git worktree list` | Shows every worktree, its path, commit and branch |
| `git worktree add <path> -b <branch>` | New folder on a new branch |
| `git worktree add <path> <existing-branch>` | New folder on a branch that already exists |
| `git worktree remove <path>` | Deletes the folder (refuses if it has uncommitted changes, unless you add `--force`) |
| `git worktree prune` | Cleans up records of worktrees whose folders you deleted by hand |

When a task is done, merge or open a pull request from that branch as usual, then remove the worktree.

## What are the gotchas with git worktree?

Most of the friction comes from the fact that a new worktree contains **only tracked files**:

1. **Dependencies aren't there.** `node_modules`, `target/`, `.venv` and friends are ignored by git, so each worktree needs its own install or build. Budget the disk space and the first-build time.
2. **Local config isn't there either.** `.env` and other git-ignored settings must be copied or symlinked into each worktree.
3. **One branch, one worktree.** Git won't let two worktrees check out the same branch. If `git worktree add` complains that a branch is already checked out, create a new branch instead.
4. **Ports collide.** Two dev servers started from two worktrees will fight over the same port unless you change it in one of them.
5. **Folders pile up.** Delete a worktree folder with `rm -rf` and git still remembers it until you run `git worktree prune`.

None of these are dealbreakers. They're just things you have to remember every single time.

## The real problem: keeping track of five terminals

Once the setup works, a different problem shows up. Five Claudes in five terminal tabs means:

- You don't know which one is **waiting for your approval** until you flip through every tab.
- You don't know which ones are **done** and need reviewing.
- You lose track of which worktree is on which branch and how far it is from `main`.
- Two of them may be **editing the same file** in different worktrees, and you only find out when you merge.

<video src="/media/video/promo-five-terminals.mp4" poster="/media/video/promo-five-terminals-poster.webp" autoplay muted loop playsinline preload="none" width="1280" height="720"></video>

Terminal multiplexers like tmux help arrange the windows, but they don't know what the AI inside each window is doing. That missing layer is what gtree adds.

## How gtree handles multiple Claude Code sessions

[gtree](https://gtree.dobbylab.com/) is a native macOS app built around worktrees. Open your repository and every worktree, including the ones you created on the command line, shows up in the sidebar.

**Creating a worktree.** In the Workbench, right-click a worktree card and choose *New Worktree…*, then type a branch name. gtree runs `git worktree add` for you and puts the new folder next to the repo, named after the branch. Removing one is also on the right-click menu, with a confirmation first.

**Running Claude in it.** The AI page has a built-in terminal for every worktree, and each one runs the real `claude` command. It's the same Claude Code you use in iTerm, so slash commands, plan mode, permission prompts and MCP all work. You can open more than one Claude tab in the same worktree when you need to.

**Seeing every session at a glance.** Each worktree in the sidebar has a status icon: a purple spinner while its Claude is working, a blinking red bell when it's waiting for you, and a faded purple robot when Claude is open but idle. You don't have to read anything; you look for the red bell. More on that in the next article.

**Knowing where each branch stands.** Each Workbench card shows the branch, how many commits are unpushed or waiting to be pulled, and how many files are uncommitted. Commit, push, pull, rebase and switching branches are on the card's right-click menu, and they run through your own git, so your SSH keys and hooks keep working. For the big picture, **Changes › History** shows every branch, each labeled with the worktree it's checked out in.

![The History graph: branches such as fix/login, refactor/api-retry and feat/dark-mode each get a colored lane, and each branch label names its worktree](/media/app/history-graph-ai-turns-light.webp "Changes › History: one lane per branch, each labeled with the worktree it's checked out in")

**Reviewing what changed.** The Compare view opens on "main → current worktree" by default, so you can review a branch before merging without typing a single `git diff`.

**Collision warnings.** If two worktrees change the same file, the Workbench shows a warning above the cards before you get to merge. Two sessions in the same worktree get a heads-up too, before one edits a file the other just changed.

### Manual setup vs. gtree

| | Terminal + git | gtree |
|---|---|---|
| Create a worktree | `git worktree add ../x -b x` | Right-click → New Worktree… |
| Start Claude | `cd` + `claude` in a new tab | Open the AI page on that worktree |
| Who's waiting for you? | Check every tab | Bell icon in the sidebar, plus a notification |
| Branch status | `git status` / `git log` per folder | One card per worktree |
| Same file edited twice | Found at merge time | Warned as soon as both have changed it |

## A simple workflow that scales to five sessions

1. Keep `main` (or your default branch) checked out in the main folder and don't let an AI work there.
2. One task, one worktree, one branch. Name the branch after the task.
3. Give each Claude a tightly scoped prompt that names the files or area it owns.
4. Review each branch against `main` before merging, then remove the worktree.
5. Merge often. The longer two branches live, the more they drift.

## FAQ

### How many Claude Code sessions can I run at once?

There's no fixed limit from git. In practice you're limited by your attention, your machine (each worktree may need its own dependencies and build) and your Claude plan's usage limits. Many people find three to five parallel sessions manageable.

### Do worktrees use a lot of disk space?

The git history is shared, so a worktree only costs the size of the checked-out files plus whatever you install or build in it. Dependency folders like `node_modules` are usually the biggest part.

### Do I need gtree to use worktrees with Claude Code?

No. Everything above works with plain git and a terminal. gtree just removes the bookkeeping: creating worktrees, finding the session that's waiting, and spotting overlapping edits.

### Does gtree replace Claude Code?

No. gtree runs the real `claude` command inside its terminal and reads its status. Your Claude Code setup, settings and subscription stay exactly as they are.

gtree is free for local use, needs no sign-up, and never uploads your code. [Download it here](https://gtree.dobbylab.com/) and open the repository you're already working in. Your existing worktrees will be waiting in the sidebar.
