# Testing notes

[English](verification.md) | [ภาษาไทย](verification.th.md)

Recorded on 2026-10-28. These notes distinguish completed checks from work still to be tested. Behavioral checks used isolated fixture projects.

## Current status

| Check | Result |
| --- | --- |
| Simple task with the current skill | Fixed an HTML typo without web research or a new skill file. |
| Documentation consistency | Reviewed the skill, README, setup guide, and metadata. Found no main instruction that contradicted researching only when useful. |
| Installation for Codex and Claude Code | Skills CLI copied the files to both agents' directories. File names and hashes matched the source package. |
| Package structure | The controller and repository maintenance skill passed `quick_validate.py`. Local links and icon paths were checked. |
| Execution in Claude Code | Not tested. Claude Code CLI was not available on the test machine. |
| Other agents | Execution has not been tested. |
| Work principles (2026-10-29) | Added eight principles adapted from Superpowers and Ponytail to OneForAll, with sources in `references/work-principles.md`, and updated the automatic-use block. Format and links checked. Whether agents follow them, and how they interact with installed Superpowers or Ponytail, has not been tested. |
| head-chef skill (2026-10-29) | Moved the model routing guide into its own skill, `skills/head-chef/`. Both skills pass `quick_validate.py`. Skills CLI installed `oneforall` and `head-chef` together into an isolated folder for Codex and Claude Code. Routing a real job, the CLI check, and the fresh-reviewer fallback have not been tested on either agent. |

## Typo correction

An independent agent read the current OneForAll skill and was asked to change `Welcom` to `Welcome` in an HTML heading. It was not told which workflow to choose.

The agent changed one word and read the file afterward to confirm the result. It made no external searches and created no additional files. The host required an initial skill-use disclosure, but there was no research report or installation recommendation.

This was one run. More request types still need to be tested.

## Keeping project knowledge separate

These two cases used an earlier version, before host adaptation and conditional research were added. They provide evidence about task completion and separate project skills, not about when the current version chooses to research.

**Atlas Contacts — CSV:** Input included Thai text, commas, double quotes, newlines, and empty fields. The original exporter used string joining, and its round-trip test failed. After switching to `csv.writer`, the same test passed. The agent created `atlas-contacts-csv-export` within the project.

**Beacon Inventory — TSV:** This project required tabs, UTF-8 without a BOM, and LF record endings. The agent read the new requirements, configured the writer accordingly, and created a separate `beacon-inventory-tsv-export` skill. Round-trip and byte checks passed.

Both cases involved separate Python-documentation and skill searches. Unavailable sources were recorded as unavailable. The generated skills passed format validation, and both projects passed a repeat test run on Python 3.11.9, with one test per project.

## Installation

Installation was exercised in a separate directory using the release checkout:

```sh
npx skills add /path/to/OneForAll --skill oneforall --agent codex claude-code --copy --yes
```

Replace `/path/to/OneForAll` with the checkout location on the test machine. The command omits `--global`, so it does not overwrite personal skills.

The installer found two skills in the repository and selected only `oneforall`, as requested. It copied the package into `.agents/skills/oneforall/` and `.claude/skills/oneforall/`. File names and hashes matched the source package.

This check covers file placement. It does not establish that the workflow has been exercised inside Claude Code.

## Still to test

The following cases do not yet have test results:

1. Choosing between existing guidance and new research across varied requests, including progress questions and tasks already covered by a skill.
2. Switching from Codex to Claude Code and reusing an existing skill without duplication.
3. Offering a third-party skill while continuing the main task, without installing it before the user chooses.
4. Correcting an existing managed skill while preserving guidance that remains valid.
5. Respecting read-only and offline constraints.
6. Handling symlinks or junctions that point outside the project.
7. Preserving concurrent changes when multiple chats edit the same file.

## Limitations

OneForAll provides instructions for an agent to follow. Behavior may vary with the model, tools, and host configuration. Permission enforcement remains the responsibility of the agent and its sandbox.

To report a problem, [open an issue](https://github.com/thefogusz/OneForAll/issues) with the request, agent version, and observed result. Remove private information before sharing examples.
