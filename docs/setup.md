# Setup guide

[English](setup.md) | [ภาษาไทย](setup.th.md)

Choose the installation for Codex or Claude Code. Both use the same skill files, with different locations and invocation commands.

## Install with Skills CLI

You'll need Node.js/npm and Git. For Codex, run:

```sh
npx skills add thefogusz/OneForAll --skill oneforall head-chef --agent codex --global
```

For Claude Code, replace `codex` with `claude-code`, or install for both at once:

```sh
npx skills add thefogusz/OneForAll --skill oneforall head-chef --agent codex claude-code --global
```

`--skill oneforall head-chef` installs both skills from this repository; list only `oneforall` or only `head-chef` to install one. `--global` makes it available across projects. For a project-only installation, run the command from that project's directory and omit `--global`.

The installer may use symlinks so multiple agents can share the same files. Add `--copy` if you prefer separate copies.

For another agent, choose its identifier from the [Skills CLI agent list](https://github.com/vercel-labs/skills/blob/main/docs/agents.md). A successful installation confirms file placement. See the [testing notes](verification.md) for what has been tried in each agent.

## File locations and invocation

| Agent | Personal skill | Invocation | New project skills |
| --- | --- | --- | --- |
| Codex | `~/.agents/skills/oneforall/` | `$oneforall` | `<project>/.agents/skills/<name>/` |
| Claude Code | `~/.claude/skills/oneforall/` | `/oneforall` | `<project>/.claude/skills/<name>/` |
| Other agents | Follow the agent's documentation | Use its supported command or ask it to read `SKILL.md` | Verify the agent's project path first |

`~` means your home directory; `<project>` means the project's root directory. Substitute the locations on your machine. In PowerShell, use `$HOME` to find your home directory; on macOS/Linux, use `echo "$HOME"`.

Some Codex installations discover skills in `$CODEX_HOME/skills` or `~/.codex/skills`. If you already have a working OneForAll installation, keep that location rather than creating another copy.

## Install manually

This method doesn't need Node.js. Clone or download the repository, then copy the entire `skills/oneforall/` folder, and `skills/head-chef/` if you want it, to your agent's personal skill directory from the table above. Keep each folder's files together.

If OneForAll is already installed, back up or compare the existing files before replacing them. Start a new session if the skill isn't visible yet.

## Enable automatic use

You can invoke OneForAll directly after installation. To let the agent decide when to use it as work begins, add the block below to your personal instruction file:

- **Codex:** `AGENTS.md` under `$CODEX_HOME`, usually `~/.codex`. If a non-empty `AGENTS.override.md` exists, add the block there because it takes precedence over `AGENTS.md`.
- **Claude Code:** `~/.claude/CLAUDE.md`.
- **One project only:** use the project's `AGENTS.md` for Codex or `CLAUDE.md` for Claude Code.

Append this block and preserve the other instructions in the file:

```markdown
<!-- oneforall:start -->
## OneForAll

On substantive project work, follow the work principles in the installed
oneforall skill: understand first, smallest complete change, root cause
before fixes, evidence before claims. Quietly assess whether current
abilities and project guidance are sufficient. If they are, do the task
directly.
Follow the installed oneforall skill when a concrete gap or a useful
specialist workflow warrants discovery, or when I request skill research.
On that route, research technical information and skills separately.
Respect explicit offline, read-only, or disabled-OneForAll requests.

I authorize automatic creation and maintenance of OneForAll-managed
skills inside the current project's native skill directory. Inspect
both .agents/skills and .claude/skills before creating anything, reuse
matching skills in place, and follow project and branch instructions.

Missing third-party skills are optional recommendations. Do not install
them without my selection or wait for that selection to continue the
main task. Keep project knowledge inside its own checkout. Surface skills
only when they materially help; skip skill chatter for routine work,
casual conversation, and status updates. Reuse suitable existing skills
without another search or an unnecessary rewrite.
<!-- oneforall:end -->
```

This authorizes OneForAll to create and update the skills it manages within the project. Installing recommended third-party skills still requires your selection.

If the agent cannot find OneForAll, add the path to its installed `SKILL.md` in the block and start a new session. For other agents, use the persistent instruction mechanism documented by that agent.

## Try it

```text
Codex:       $oneforall Check and fix this project's export.
Claude Code: /oneforall Check and fix this project's export.
```

If existing capabilities and guidance cover the task, the agent can proceed directly. There may be no research or new skill file. When further research would help, it searches for technical information and skills separately, then saves useful procedures in the project.

To request skill discovery directly, try: "Find skills that could reduce repetitive work in this project, and explain why they fit."

When switching between Codex and Claude Code in the same project, OneForAll can read an existing skill directly from the other directory without creating a duplicate. Each agent's automatic discovery still depends on the paths that agent supports.

## Disable or uninstall

- **For one task:** say "Don't use OneForAll for this task," or set a constraint such as "Read only; don't change files."
- **Disable automatic use:** remove only the block from `oneforall:start` through `oneforall:end` in the instruction file you edited.
- **Uninstall:** disable automatic use first, then use your installer to remove the controller or remove its folder manually. Project skills remain available for reuse.

## Other environments

OneForAll uses the Agent Skills format. It needs file tools to read and write skills, plus web tools when research is required. In a chat without persistent file storage, it can only draft a skill for you to save. A cloud environment may need a separate installation from your local machine.

Paths and commands were checked against documentation on 2026-10-28: [Codex skills](https://learn.chatgpt.com/docs/build-skills), [Codex instructions](https://learn.chatgpt.com/docs/agent-configuration/agents-md), [Claude Code skills](https://code.claude.com/docs/en/skills), [Claude Code memory](https://code.claude.com/docs/en/memory), [Skills CLI](https://github.com/vercel-labs/skills/blob/main/docs/commands.md).
