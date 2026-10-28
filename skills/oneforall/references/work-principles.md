# Work principles: sources and reasoning

The eight principles in SKILL.md are a compact adaptation, written in our own words, of two MIT-licensed projects. Neither needs to be installed. When one is installed, its full skills give the detailed procedure, and OneForAll does not duplicate them.

- [Superpowers](https://github.com/obra/superpowers) by Jesse Vincent (MIT), read at commit `2ce8aa1` on 2026-10-29: skills that trigger automatically for brainstorming, planning, test-driven development, systematic debugging, verification before completion, and writing skills with tests.
- [Ponytail](https://github.com/dietrichgebert/ponytail) by DietrichGebert (MIT), read at commit `09c434a` on 2026-10-29: a "lazy senior developer" prompt that writes only what the task needs, reuses what exists, and never cuts safety.

## Where each principle comes from

| Principle | Source idea | What we adopted | What we left out |
| --- | --- | --- | --- |
| 1. Understand before acting | Ponytail "before you write"; Superpowers brainstorming | Read what the change touches and list every place it must reach. Confirm intent for new or ambiguous work. | Superpowers' full design-document flow for small changes. Large new projects can still use the brainstorming skill if installed. |
| 2. Smallest complete solution | Ponytail's ladder | The ladder, extended with "a project skill" as something already in the project, so skills are reused before new ones are written. | Ponytail's tone and intensity modes. |
| 3. Never cut | Ponytail's "never cut" list | The list as is. | — |
| 4. Root cause before fixes | Superpowers systematic-debugging; Ponytail "grep every caller, fix once" | Reproduce, find the cause, fix in shared code; stop after repeated failed fixes and question the design. Superpowers uses three failed fixes as the signal; we use two to keep cost down. | The four-phase checklist detail. |
| 5. Test what can break | Superpowers TDD; Ponytail "one small test" | Ponytail's proportional rule (non-trivial logic only) with Superpowers' "see it fail first" when practical. | Superpowers' rule to delete code written before its test; strict TDD stays available through that skill. |
| 6. Small verifiable steps | Superpowers writing-plans | Steps that each end in a check. | Fixed step sizes and plan documents for every task. |
| 7. Evidence before claims | Superpowers verification-before-completion; Ponytail's closing line | Run the check now, read the output, then claim. One line on what was skipped or unverified. | Ponytail's closing line on every reply; OneForAll only adds it when something was skipped. |
| 8. Mark deliberate shortcuts | Ponytail `shortcut:` comments | The comment form, so a later task can find them. | The `/ponytail-debt` ledger command. |
| Skills tested like code (section 4) | Superpowers writing-skills ("no skill without a failing test first") | Name the baseline failure before writing a project skill; check the same situation afterwards. | Subagent pressure-test suites for every skill; worth it for discipline skills that agents tend to skip. |

## Resolving conflicts

- **User and project instructions** come first, then any installed process skill that is stricter, then these principles.
- **Strict TDD vs proportional tests:** when the Superpowers TDD skill is installed and applies, follow it. Otherwise use principle 5.
- **Quiet reporting vs closing lines:** OneForAll keeps reports short. The one-line "skipped or unverified" note appears only when something actually was.
- **Cost:** principles 1, 4 and 7 add reading and checking, which costs tokens but avoids rework. Principle 2 and the head-chef companion skill cut cost elsewhere. Do not skip a check to save tokens.
