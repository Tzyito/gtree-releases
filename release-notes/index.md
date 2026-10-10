# Release notes

What changed in each version. If you already have gtree, the sidebar shows “Update and restart” when a new version is out.

## v0.3.9 (2026-10-10) — Codex support, agents that sort it out between themselves, and Teammates

### New
- Connect Codex: click Connect Codex in Settings and Codex sessions work like Claude Code ones. You can see what each one is doing and when it's waiting for you, waiting sessions show up in Inbox, and every turn is saved so you can roll it back. No need to approve anything inside Codex.
- The + in the AI view can open a Codex session next to your Claude Code ones in the built-in terminal, and History now finds and replays Codex conversations too.
- Agents in the same worktree, Claude Code or Codex, now know about each other: when one changes a file the other also changed, the other is told automatically, and before touching the same place they leave each other a message to agree who goes first. Whoever finishes first tells the other.
- New Teammates, off by default and turned on per repository: in repositories where someone else has committed recently, a Teammates section appears under the project in the sidebar. Click Turn on, then Start sharing, and you and your teammates see each other's work in progress, uncommitted changes included, through your team's own git repository. Click a branch to see their changes.
- When you and a teammate change the same place, your AI stops and asks you before editing, and Inbox flags it with a See their changes button.
- Your AI can leave a teammate a message. It reaches their AI only after they have read it and clicked Hand to my AI, and the reply comes back to the AI that asked.
- Everything between teammates is signed and messages can only be read by their recipient, so no one else with access to the repository can pose as a teammate. If a teammate's identity changes, nothing from them is accepted until you confirm it's really them.

### Improved
- Clicking "AI open in another terminal" in the status bar now lists those sessions: what was asked, how long ago and what it's doing. Click one to switch to its terminal, or, if gtree can't tell where it runs, to replay the conversation.
- Right-click in the file tree to Open in Browser, Open with Default App, Reveal in Finder, Open in Terminal (gtree's built-in terminal) or Copy Path.

### Fixed
- Paging through the welcome tour no longer flashes other screens, and its last page now shows how to connect Claude Code.
- When long commands run in parallel in one worktree, files are no longer credited to the wrong session, and "0 failed" in test output is no longer taken for an error.
- The Claude Code connection text no longer mentions error or stuck states the app never shows.

## v0.3.8 (2026-10-09) — An Inbox, native notifications and four-item navigation

![The gtree Inbox: sessions waiting for you at the top, each with Open; under Worth a look, two worktrees changing the same file; the AIs currently working at the bottom](/media/app/inbox-light.webp "The new first screen, Inbox: who's waiting for you and where work overlaps, on one page")

### New
- Inbox is the new first screen and answers one question: does anything need you right now? AIs waiting for you come first, sorted by how long they've waited, followed by things worth a look, such as two branches changing the same file.
- Get a macOS notification when an AI stops to wait for you, whether it needs permission or has finished and is waiting for your reply; click it to jump to that session. The Dock icon shows how many things are waiting.
- When two Claude Code sessions work in the same worktree, an AI is warned before it edits a file the other just changed, so they don't overwrite each other.
- Remove gtree from this Mac right from Settings: it disconnects Claude Code, deletes gtree's data (or keeps it if you plan to reinstall) and moves the app to the Trash.

### Improved
- Navigation is down from seven items to four: Inbox, Workbench, Changes and AI, on ⌘1–⌘4. Changes holds AI changes, History and Compare.
- Connecting Claude Code takes one click from the welcome tour, the Workbench banner or the empty AI changes page, and asks for notification permission at the same time. No Terminal commands.
- Compare opens with something to show: the main branch versus your current worktree.

### Fixed
- Repositories with hundreds of branches no longer slow your whole Mac down.
- Closing a terminal tab now ends the Claude running inside it, and several Claude tabs in one worktree no longer overwrite each other's status.
- Updating and connecting Claude Code no longer fail when gtree runs straight from your Downloads folder, and gtree offers to move itself to Applications in one click.

## v0.3.7 (2026-10-09) — Move gtree anywhere without breaking Claude Code

### Improved
- Moving gtree.app to another folder no longer quietly breaks its connection to Claude Code; existing connections switch over automatically on first launch.

### Fixed
- The install command no longer fails with “unbound variable” in some terminals, an error that stopped many new users from installing since 0.3.3.

## v0.3.6 (2026-10-09) — New installs counted correctly in usage statistics

### Fixed
- New installs are now counted in anonymous usage statistics on their first day — still only after you've seen the notice, and you can turn statistics off in Settings.

## v0.3.5 (2026-10-09) — Reliable first launch for browser downloads

### Fixed
- Opening gtree from a .zip downloaded in a browser no longer reports “gtree is damaged and can't be opened”: the app is now fully signed, so macOS only asks you to allow it once (System Settings → Privacy & Security → Open Anyway).

## v0.3.3 (2026-10-09) — Set up the Claude Code integration from Settings

![The Claude Code connection section in gtree Settings: Not connected, a Connect Claude Code button, and a short explanation of what connecting does](/media/app/settings-claude-not-connected-light.webp "Connect Claude Code from Settings in one click, with what it does and what it changes explained right there")

### New
- Settings has a new Claude Code integration section: install in one click, see in plain words why it helps and what it changes, pick the install folder, review and switch off each hook, or uninstall.
- The Workbench shows a banner when the Claude Code integration isn't installed or needs an update, linking straight to Settings.
- Drag AI tabs to reorder them; the tab bar scrolls when there are more tabs than fit.
- Settings previews the upcoming Team plan, with expected pricing and an early-bird list.

### Improved
- The Timeline explains what it's for, lists only turns that changed files by default, and shows system messages and Markdown as readable text.
- The sidebar, file panel and AI history panel now open and close with a smooth animation.
- Command-line output and `gtree --help` follow your interface language.

### Fixed
- Sidebar content no longer slides over the window buttons while the sidebar opens or closes.
- Past conversations no longer stay stuck as “open in another terminal”, and can be resumed again, after Claude Code subagents finish.

## v0.3.2 (2026-10-09) — gtree now speaks English and Japanese, plus a welcome tour

![The General section of gtree Settings: a language switch with English, 日本語 and 中文, and a Replay button for the welcome tour](/media/app/settings-general-notifications-light.webp "Switch the interface language any time in Settings, and replay the welcome tour from the same place")

### New
- The interface is now available in English, Japanese and Simplified Chinese; it follows your macOS language and can be switched in Settings, taking effect immediately.
- A gear button at the bottom-left of the sidebar opens Settings and shows the current language.
- New users get a six-page welcome tour on first launch; replay it any time from Settings → General.

### Improved
- The welcome tour respects macOS “Reduce motion”, fading instead of animating.
- The install command now prints its progress in English.

### Fixed
- Panel titles on the Workbench are no longer cut off in English and Japanese.
- Network view cards no longer show raw system tags in place of what you asked.

## v0.3.1 (2026-10-08) — New Settings panel with a usage statistics switch

### New
- Added Settings (⌘, or gtree → Settings… in the menu bar); its first option turns anonymous usage statistics on or off at any time.

### Improved
- The first-launch notice about usage statistics is written in plainer language and points you to Settings.

## v0.3.0 (2026-10-08) — Timeline — review and restore every AI turn

![The AI changes page: turns that changed files in the middle; on the right, the selected turn's prompt, Claude's reply, the changed files and their diff, with Restore to this turn in the corner](/media/app/ai-changes-timeline-light.webp "Every turn's prompt, reply and changes are kept, and Restore to this turn puts that turn's files back")

### New
- Timeline replaces Snapshots: after each Claude Code turn, gtree records your prompt, the reply and the files that changed, so you can review each turn's diff and restore the files that turn changed without touching your branch or staging area.
- New Claude Code sessions start knowing where the last session in the same worktree left off: what was asked, the final reply and which files changed.
- The AI can look up earlier turns by itself, including turns from your other projects.
- The commit graph can show how each AI turn ended, as a branch growing from your current commit; show or hide it from the toolbar.
- The Network view opens with a card for each active AI, showing what it's working on and what it did last turn, with the ones waiting for you first.
- “Take a look” jumps straight to the Claude working in a worktree: its AI tab if it runs in gtree, or a read-only replay if it runs in your own terminal.

### Improved
- Downloads and updates go through gtree's own download server, falling back to GitHub automatically if it can't be reached.
- gtree now collects anonymous usage statistics and tells you on first launch before anything is sent; turn them off from that notice or with GTREE_NO_TELEMETRY=1.
