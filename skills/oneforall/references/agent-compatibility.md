# Host adaptation

Use the same SKILL.md workflow with the active host's tools and project conventions. A model called Claude inside another agent does not make that host Claude Code.

| Host | New project skills | Personal controller | Explicit invocation |
| --- | --- | --- | --- |
| Codex | `.agents/skills/<name>/` | `~/.agents/skills/oneforall/` or the existing location actually discovered by Codex | `$oneforall` |
| Claude Code | `.claude/skills/<name>/` | `~/.claude/skills/oneforall/` | `/oneforall` |
| Other Agent Skills clients | Verify the host's project path from its docs/config | Use its installer or documented personal path | Use its native command or ask it to read OneForAll's SKILL.md |

`~` means the current user's home. These paths are conventions, not literal universal absolute paths. Some installed Codex profiles expose skills under `$CODEX_HOME/skills`; use the location already discovered instead of creating a second copy. `agents/openai.yaml` supplies Codex UI metadata; the workflow itself does not require it.

## Switch agents without cloning knowledge

Always inspect the active checkout's `.agents/skills` and `.claude/skills` before creating a skill. When a matching project skill exists in either, read it directly and reuse it there. If it is OneForAll-managed and needs a change, patch that copy. This controller can read a file outside its host's automatic discovery folder; that does not imply the host natively discovers it. Do not create mirrors or symlinks automatically. Respect the existing containment check on every read of a candidate and every write.

For a host without documented native skill support, an agent with file access can read the controller explicitly and keep project guidance in `.agents/skills`, but report that automatic discovery is unverified. If the agent cannot write persistent project files, provide the proposed skill as an artifact and state that persistence is unavailable. Do not pretend chat-only or cloud sessions share the local filesystem.

## Tools and permissions

Use available native read/search/edit/shell tools; do not require Codex tool names, Claude executables, or a particular MCP server. Web search/fetch access is needed for the two external research tracks. If absent, follow the controller's honest local-evidence fallback. `skill-creator` and its validator are optional helpers; check ordinary frontmatter and links directly if unavailable.

Keep normal host permission prompts, sandbox restrictions, project instructions, and user decisions. OneForAll does not bypass them. Do not add hooks, grant permissions, change personal instruction files, or install third-party tools during ordinary project tasks. Setup of automatic invocation is a separate user-directed action.

## Automatic invocation setup

For Codex, append the routing instructions to the active personal instructions file under `CODEX_HOME` (default `~/.codex`); an existing non-empty `AGENTS.override.md` takes precedence over `AGENTS.md`. For Claude Code, use the personal `~/.claude/CLAUDE.md`, or project `CLAUDE.md` when only that project should opt in. Preserve existing contents. For other clients, verify their supported always-loaded instruction mechanism rather than guessing that either filename is honored.

An instruction block improves consistency; it is not a deterministic per-message hook. Neither installing this skill nor adding `allow_implicit_invocation` proves that it will run every time.

## Sources checked 2026-10-28

- [Codex skills](https://learn.chatgpt.com/docs/build-skills)
- [Codex instructions](https://learn.chatgpt.com/docs/agent-configuration/agents-md)
- [Claude Code skills](https://code.claude.com/docs/en/skills)
- [Claude Code memory and instructions](https://code.claude.com/docs/en/memory)
- [Skills CLI agent paths](https://github.com/vercel-labs/skills/blob/main/docs/agents.md)
- [Agent Skills client integration](https://agentskills.io/client-implementation/adding-skills-support)
