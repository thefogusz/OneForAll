---
name: oneforall-release-docs
description: Prepare or review public documentation and skill releases in the OneForAll repository, checking installation, runtime compatibility, and evidence claims.
metadata:
  oneforall-managed: "true"
---

# OneForAll release documentation

Apply only to this repository, identified by its OneForAll controller under `skills/oneforall/`. Recheck the tree before reuse.

- The distributable controller is `skills/oneforall/`. This maintenance skill is for contributors and is not needed by end users; installer examples must select `--skill oneforall`.
- Keep README examples portable: `~`, `$CODEX_HOME`, and `<project>` must be explained. Exclude personal absolute paths, local instruction backups, transcripts, and discarded artwork from publication.
- Write public documentation in natural, professional Thai. Use descriptive headings, explain the problem and usage directly, and keep testing detail in its dedicated page. Avoid forced jokes, slogans, and fist/combat metaphors; the logo is not a writing theme.
- Compare every runtime claim with `skills/oneforall/references/agent-compatibility.md` and current official docs. Distinguish native discovery, manual loading, installer placement, and observed execution.
- Keep the quiet capability assessment, conditional two-track research, and non-blocking recommendations clear. Routine work uses existing capabilities directly. A generated skill should improve a task; an optional installation suggestion must not halt it.
- Validate the package using an available skill validator, check relative Markdown/HTML asset links, and exercise installation in an isolated local project for the targeted agents. Do not install test copies into personal skill directories.
- Review the staged diff, then push only when the user authorizes it. Verify the remote commit after pushing. Never claim a test was run just because a test plan exists.

## Verification

Public-release checks are reported in `docs/verification.md`. Re-run checks relevant to changes before updating that report.

## Research checked 2026-10-28

Information: [GitHub README behavior](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-readmes), [Codex skills](https://learn.chatgpt.com/docs/build-skills), and [Claude Code skills](https://code.claude.com/docs/en/skills). Adopt relative links and host-specific discovery paths.

Skill research: [Anthropic documentation skill](https://github.com/anthropics/knowledge-work-plugins/blob/main/engineering/skills/documentation/SKILL.md) was read after a separate GitHub SKILL.md search. Adopt reader-first quick starts, concrete examples, and accurate links; no installation required. Local documentation-and-adrs and code-review-and-quality guidance already cover broader documentation and review needs.
