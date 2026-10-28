# Research routes and design sources

OneForAll combines discovery, project adaptation, and experience-based maintenance. These are independent techniques; none of the projects below needs to be installed to use this controller. This is original orchestration guidance, not a vendored copy of their skills.

Use these routes only after the controller identifies a concrete benefit from additional skill research, or the user requests discovery. Routine work and already-covered tasks do not need an external search.

## Two independent searches

| Track | Starting point | Follow-up |
| --- | --- | --- |
| Technical information | Official docs for the actual framework/version, primary papers, vendor examples | Read the relevant sections; compare with local implementation and constraints. |
| Existing skills | Local catalog/project skill roots for the active host, then skills.sh | Read the candidate's source `SKILL.md`, not only its listing. |
| Broader skill discovery | Source repositories and official vendor skill collections | If no fit appears, try a synonym or another collection before concluding none was found. |

Example distinct queries for a Python CSV export problem:

- Information: `site:docs.python.org/3/library/csv.html newline encoding`
- Skills: `site:skills.sh python csv export`
- Skill fallback: `site:github.com "SKILL.md" "csv" "Python"`

Use actual project versions in queries when that matters. Search terms are examples, not commands to execute literally. Search installed skills by their descriptions and relevant file contents instead of loading every skill.

## Building blocks

**Vercel find-skills — external discovery.** Search the ecosystem, open source skills, and compare applicability. Use the connected web/search tools or browse skills.sh directly. If the Skills CLI is already available, its `find` command is another route. `npx skills find <query>` can download/execute a package, so it is not required for read-only discovery. Do not run `skills add`, global installs, or bulk updates merely to read a skill.

- [Vercel Agent Skills](https://vercel.com/docs/agent-resources/skills)
- [find-skills source](https://github.com/vercel-labs/skills/blob/main/skills/find-skills/SKILL.md)
- [skills.sh directory](https://skills.sh/)

**AutoSkill — useful memory and deduplication.** Borrow the distinction between reusable lessons and incidental conversation, and choose between reuse, improvement, and creation. Its full SDK, proxy, database, and background services are unnecessary here. OneForAll's user-authorized project maintenance and conditional discovery behavior are defined by the controller, rather than AutoSkill's default approval and after-session workflow.

- [AutoSkill](https://github.com/ECNU-ICALK/AutoSkill)
- [AutoSkill local manager](https://github.com/ECNU-ICALK/AutoSkill/blob/main/skills/autoskill/SKILL.md)

**OpenAI skill-creator and skills — local packaging.** Consult the installed `skill-creator` for concise instructions, resources, validation, and UI metadata. Search the OpenAI collection for relevant workflows; verify that a candidate's tools exist in this environment.

- [OpenAI skills collection](https://github.com/openai/skills)
- [Codex local skills](https://learn.chatgpt.com/docs/build-skills)
- [Codex AGENTS.md discovery](https://learn.chatgpt.com/docs/agent-configuration/agents-md)

**Anthropic skill-creator and skills — authoring and realistic evaluation.** Consider task-oriented descriptions and testing a skill on representative requests. Translate runtime-specific tooling to the current environment; do not assume Claude executables exist. The collection is also an independent skill discovery source.

- [Anthropic skills collection](https://github.com/anthropics/skills)
- [Anthropic skill-creator](https://github.com/anthropics/skills/blob/main/skills/skill-creator/SKILL.md)

**Superpowers and Ponytail — how to work, not what to find.** The work principles in SKILL.md adapt Superpowers' process discipline (understand, plan, test, debug by root cause, verify before claiming, test skills) and Ponytail's smallest-complete-change ladder and never-cut list. Their skills are also good recommendation candidates when a project needs the full procedure. Details: [work-principles.md](work-principles.md).

- [Superpowers](https://github.com/obra/superpowers)
- [Ponytail](https://github.com/dietrichgebert/ponytail)

**Agent Skills specification — portable structure.** Use a lowercase folder/name, required frontmatter, focused instructions, and linked references as needed.

- [Agent Skills specification](https://agentskills.io/specification)

The controller's design sources were read on 2026-10-28. This date does not imply that a candidate for a later project task has been checked. When discovery is warranted, research that gap separately, recording its actual evidence and date.

## Evidence handling

For each candidate actually read, retain its source and revision/version if available, the part relevant to the project, and the reason for using or rejecting it. Distinguish `no suitable candidate in the searched sources` from `search unavailable`. Keep information and skill evidence separate even if both ultimately come from the same vendor.

New project skills need enough provenance to recheck their claims, not copies of entire web pages. Inspect bundled scripts before adapting any executable behavior. Follow licensing requirements for copied material, and keep unverified external claims separate from successful local checks.
