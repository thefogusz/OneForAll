---
name: head-chef
description: Route each step of a recurring model job to the cheapest station that can do it - code, an optional classifier, then small, mid and strong models - with tiers resolved from the host's current model lineup. Use when building, downgrading, or auditing a scheduled job, pipeline, repeatedly run skill, or multi-agent workflow that calls a model, or when automated jobs strain cost or usage limits. Works on Claude Code and Codex. Not for one-off interactive work.
---

# head-chef — the strong model writes the recipe, cheaper ones cook

Use this when the task builds, downgrades, or audits work that calls a model repeatedly: a scheduled job, a pipeline step, a skill that runs many times, or a multi-agent workflow. One-off interactive work does not need it.

## Quick start

1. Split the job into steps (input → output).
2. Give each step to the first station that can do it: code → classifier → prep → sous → head chef.
3. Resolve which real model each tier means today ("Resolving tiers at run time").
4. Have the head chef write the recipe card: scripts, prompts with schemas, code checks, replay set, escalation rule, budget.
5. Prove any downgrade by replay, ship it as a reversible switch, and watch it with sampled reviews.
6. Write the routing card into the job's doc.

The idea: the strongest model writes the recipe once; cheaper models and plain code run it every time inside the limits that recipe sets. The cost may be money or a **shared usage allowance**. Scheduled calls to the top model draw on the same rate limits as interactive work, so a busy night of them can make other jobs fail.

## Stations

Stations are tiers, not product names. Map each tier to the active host's current lineup with "Resolving tiers at run time" below.

| Station | Tier | Does | Never |
| --- | --- | --- | --- |
| Kitchen equipment | **no model**: scripts, SQL, ffmpeg, jq | Numbers, joins, formatting, encoding, uploads, retries, schedules | Judgement |
| Gatekeeper (optional) | **typed classifier API**, if one is available | Typed decisions only (yes/no, pick-one, score) as a calibrated probability | Any text; extraction; forecasts (its probability is a gate signal, never a reported %) |
| Prep cook | **smallest model** | Yes/no checks, tagging, extraction to a schema, dedupe, keep-alives, collectors | Text the public reads |
| Sous chef | **mid model** | Runs a written recipe: reports, briefings, drafts, code to a spec, templated posts | Inventing the recipe |
| Head chef | **strong model** | Writes recipes, designs series, first build of a job, the escalation path | Running the same recipe every week |
| Consultant | **strongest model on the plan** | Only when the head chef has failed to write a working recipe | Anything recurring |
| Food critic | **another model family**, or a fresh reviewer agent (see below) | Independent review, run by hand | Cooking; any scheduled job |

"Top two models" below means the consultant and head-chef tiers.

**The kitchen equipment row matters most.** If code can do a step, code does it. A templated video clip is a template filled by code and encoded by ffmpeg, not an agent per clip. Numbers always come from scripts, never from a model.

## Resolving tiers at run time

This guide never names a model. Model lineups change faster than skills, so resolve the tiers fresh each time a job is built or audited, from what the host offers **now**.

**1. List what is actually available**, read-only, in this order. Stop at the first source that works:

- the host's own model list or picker (Claude Code's model aliases and `/model` list; the models Codex's config and picker expose);
- the provider's models endpoint, if the project already has API access (e.g. a `GET /v1/models` call with the project's existing key);
- the provider's official models/pricing page, read with the web tools.

Drop anything marked deprecated, preview-only, or not available to this account. Never invent an ID or reuse one remembered from another conversation.

**2. Rank by price, then by the vendor's own positioning.** Within one provider, list price per million tokens (input + output) is the most stable signal of tier: vendors price small/fast models low and flagships high. Break ties, or fill gaps when prices are hidden, with the vendor's description ("fast", "efficient" → lower; "most capable", "flagship", "frontier" → higher). Benchmarks are a tiebreaker only. A larger version number alone does not mean a higher tier.

List price only sets the starting tiers. Once a replay has run, compare models by **measured cost per accepted output**: tokens actually used (including reasoning tokens) × price, plus retries and escalations, divided by outputs that passed the checks. A model that is cheap per token but thinks long, or fails checks often, can cost more than the tier above it.

**3. Assign tiers from the ranking:**

| Ranking position | Tier |
| --- | --- |
| Cheapest generally available model | prep |
| Next one up | sous |
| The vendor's flagship (most capable model in normal use) | head chef |
| Anything priced or positioned above the flagship | consultant (absent if nothing is) |

With only two models, the cheaper one is both prep and sous and the other is the head chef. With one model, use reasoning-effort or thinking settings as the rungs: lowest for prep, default for sous, highest for the head chef. Codex's `model_reasoning_effort` is such a rung on any lineup.

**4. Aliases vs pinned IDs.** Some hosts offer aliases that follow the newest model of a tier (Claude Code accepts `haiku`, `sonnet`, `opus`). An alias upgrades silently, and replay evidence belongs to the exact model it was run on. So:

- **pin the resolved ID** for steps whose errors are public or legal, and for any step that was downgraded on replay evidence;
- an **alias is fine** for prep steps whose output is fully checked by code, and for the head-chef escalation path.

**5. Re-check when the lineup changes.** When step 1 shows a model the job's routing card doesn't list, or a pinned ID is announced for retirement, propose a re-check: run the replay protocol for the affected steps. A new cheaper model may now pass a step that needed the sous tier, and a retired ID must move before it breaks. Do not swap a pinned ID without that replay.

Record the resolved tier → ID table, the source used, and the date in the job's routing card.

## Where the model is set

| Host | Where |
| --- | --- |
| Claude Code | Subagent: the Agent tool's `model` parameter. Skill: frontmatter `model` where supported. Headless or scheduled run: `claude -p --model <id>`. |
| Codex | Run: `codex exec -m <model>`. Persistent: `model` / `model_reasoning_effort` in `config.toml` under `CODEX_HOME`, or a named profile per job. |
| Other agents | The host's documented model setting; ask if none is documented. |
| Pipeline code (any host) | One env var per step (e.g. `MYJOB_SUMMARY_MODEL`) whose default is the current model, so rollback = delete the line. |

Name the station in a comment wherever the model is set, so the next reader knows it was chosen on purpose.

## Food critic

The critic reviews a recipe, a prompt, or an output it did not write.

**First, check what is connected.** Before choosing a critic, look for the other family's CLI with read-only commands only:

| Active host | Other family | Installed? | Signed in? |
| --- | --- | --- | --- |
| Claude Code | Codex | `codex --version` | `codex login status` |
| Codex | Claude Code | `claude --version` | the CLI's documented status command; if none, treat sign-in as unknown |
| Any | Another family's CLI or API the project already uses | its version/status command | its documented status command |

Then **propose** the result to the user before using it, in one short line, alongside progress on the task:

- Installed and signed in → offer it as the critic, e.g. "Codex CLI is connected; use it for a cross-family review of this recipe?" Run it only after the user agrees, since it spends that provider's quota and sends the artifact to another service.
- Installed, sign-in unknown or missing → say so and offer to use it once the user signs in. Never run a login flow or handle credentials.
- Not installed → mention it as an optional recommendation (name, benefit, cost). Never install it. Continue with the fallback below meanwhile.

Do not block the task waiting for the answer; the fallback critic can run while the user decides. Remember a decline for the rest of the conversation.

Prefer, in this order:

1. **A different model family**, once the check above shows it connected and the user agreed. On Claude Code, Codex (e.g. `codex exec`); on Codex, Claude (e.g. `claude -p`). Different families make different mistakes, which is the point.
2. **No other family connected, or the user declined → a fresh critic agent.** Spawn a new subagent on the strongest tier the host allows, with:
   - a fresh context: give it the artifact, the acceptance checks, and the replay disagreements, **not** the author's reasoning or chat history;
   - an explicit adversarial role: "You did not write this. Find what is wrong, missing, or risky. Do not fix it.";
   - a typed output: findings with severity and evidence, or an explicit "no findings".

   Label the result a **same-family review** in the evidence note. It catches context bias and sloppiness, but shares the family's blind spots, so do not treat it as equal to a cross-family check on public or legal steps.
3. **No subagent support → a separate pass** in the same session after a clean break: re-read only the artifact and checks, answer the same adversarial brief, and label it a **self-review**.

Calling another family's CLI is an external tool call: use it only when it is already installed, signed in, and accepted by the user. Never install it or sign in automatically. Sending work to another provider shares it with that provider: remove secrets, credentials, and personal or customer data from what the critic receives, or use the fresh-agent fallback for material that must not leave the current provider.

## Cheaper without changing the cook

Try these before, or alongside, a downgrade. They risk less quality than swapping models:

- **Prompt caching** for a long recipe or reference block repeated on every run, where the API supports it.
- **Batch processing** for scheduled work that doesn't need an answer within minutes, where the API offers it.
- **Lower reasoning effort** on the same model for steps that don't need deep reasoning.
- **Fewer calls**: a code precheck that only wakes the model on a fault.

## New job

1. **Split the task into steps.** One line each: input → output.
2. **Assign each step to a station**, top down, stopping at the first that can do it:
   - Can code do it? Equipment.
   - A yes/no, pick-one, or graded question about given text, and a classifier is available? **Classifier first** as a cascade: it decides the confident ends (e.g. p ≤ 0.2 or ≥ 0.8); the uncertain middle, or a classifier error, goes to the prep or sous tier. An outage routes to the model, never fails open. No classifier → go straight to the next rung.
   - Extraction to a schema, or a closed question? Prep tier.
   - Composition that follows a written recipe? Sous tier.
   - Head chef only for design, or where the recipe can't be written yet. That is a reason to write it, not to run the strong model forever.
3. **The head chef writes the recipe card once**, stored with the job inside its own project, never in a global skill folder. A recurring job's recipe is project knowledge: save it as a project skill named `<project-slug>-<job>` in the project's native skill directory (`.claude/skills/` on Claude Code, `.agents/skills/` on Codex), checking both directories first and reusing a match in place. When OneForAll is installed, follow its naming, containment, and `metadata.oneforall-managed` rules. A repo doc or notes file is fine when the project has no skill directory. The card holds:
   - **Scripts** for every deterministic step.
   - **Prompt + output schema** for each model step. Inputs go in delimiters (e.g. `<<< >>>`) as data, never instructions. Delimiters only lower injection risk; the checks below are the real guard.
   - **Checks** run by code: required fields, counts, language, disclosures, word limits, numbers matching the script's numbers, and policy rules.
   - **Replay set:** at least 20 real past inputs with accepted outputs to start, growing toward ~90 as runs accumulate. Without it a step can't be downgraded later.
   - **No history yet?** Bootstrap the replay set by running the head chef on ~20 real inputs, reviewing the outputs, and marking them `provisional`. Replace them with accepted production outputs as they arrive. Never downgrade a step on provisional cases alone.
   - **Escalation:** a check fails → retry once on the same model → head chef. Log every run as one JSON line, so the ceiling below is computed by code, not by reading:
     `{"ts": "...", "job": "...", "step": "...", "model": "...", "outcome": "pass|retry_pass|escalated|failed", "in_tokens": 0, "out_tokens": 0}`
   - **Budget guard:** a hard cap per run (calls or tokens) and per day for the job. Hitting it stops the job and alerts the owner instead of draining the shared allowance. A kill switch (env var or config line) disables the model steps without a deploy.
   - **Escalation ceiling:** more than ~10% of a week's runs reaching the head chef means the recipe is broken. Stop and send it back for a rewrite; don't let the strong model quietly cook every night.
   - **Strict stop fields:** a field that halts the pipeline (e.g. `blockers`) gets a schema description saying exactly that, and questions for the head chef get their own field. Otherwise a cheap model lists notes as blockers and halts the run.
   - **Output language** stated.
4. **Resolve tiers and wire the model**: resolve the tier → ID table as described above, then set the model where the host table says, with the station named in a comment.
5. **One head chef per job.** Never fan out several strong-tier agents to do prep. Fan out prep or sous workers under one head-chef orchestrator, and only when the user asked for multi-agent work.

## Existing job (downgrade or audit)

0. **Is it worth proving?** Estimate calls per month × price difference, or the job's share of the usage allowance. A replay costs tokens and an hour of reading. If three months of savings are smaller than that, use the "cheaper without changing the cook" levers or leave the job alone.
1. **Inventory** every scheduled job with its engine and model, and flag:
   - every job on the top two tiers (a head chef cooking on a schedule);
   - every sous-tier job whose name or prompt looks like prep work (collect, keep-alive, refresh, health, check, sync, rollup, ping, watch, prune, backup, ingest). Split ungated (model runs every tick) from precheck-gated (a script runs every tick, the model wakes only on a fault). Ungated first.

   A flag is a question for the replay protocol, not a verdict.
2. **Find where the model is actually called.** Grep for model-ID prefixes, `--model`, `-m`, `model=`, `model =` in config files, and the client's call function. Defaults count: `os.getenv("X_MODEL", "<strong-model>")` with no override on the host means the strong model runs every time.
3. **Prove the downgrade with the replay protocol:**
   - a. Collect real past outputs or verdicts from the current model (caches, logs, state files). Pull inputs **read-only**, with a least-privilege database user.
   - b. Re-ask the same inputs through the job's **own** functions with only the model swapped, from a clean checkout of the main branch.
   - c. Re-run a stratified subset (~90) on the **current** model. Its disagreement with itself is the noise floor.
   - d. Score agreement **and the direction of each error**. In news dedupe, a wrong "same story" drops real news; a wrong "different" repeats an item. Equal agreement can carry very different risk.
   - e. Read every disagreement on the costly side and name the pattern.
   - f. **Pass bar:** cheaper-model agreement ≥ current-model self-agreement − 2 points, **and** no more costly-side errors than the current model made against itself. Decide per step. If one pattern fails, the head chef rewrites the prompt to name it, then replay again.
   - g. For a yes/no step with a classifier available, replay it there too. Score AUC and pick the grey zone from a held-out half, not in-sample.
4. **Keep the strong model, or a human read, where an error is public or legal:** defamation risk, numbers on a public site, broadcast or published scripts. Evidence beats the table: if a blind test shows the sous tier clearly below on a style judgement, the step stays until a better recipe passes a re-test. Use the food critic here.
5. **Ship as a reversible switch:** an env var or config line with the old model as default. Confirm with the owner before changing anything live. Record the evidence note and rollback line where the team tracks open work.
6. **Watch it after shipping.** A replay proves the step on past inputs; inputs drift and aliases upgrade. Each week, send a small random sample of production outputs (2–5%, at least 10) to the head chef or the food critic with the same acceptance checks, and track the escalation log. Roll back when the sample's costly-side errors or the escalation rate pass the bars set in 3f and the escalation ceiling.

## Routing card

Add this to the job's doc or handoff:

```text
Job: <name>                  Host: <claude-code|codex|...>   Owner: <who>   Output language: <lang>
Step                 Station      Model / tool (ID, date checked)   Check that gates it
1 pull rows          equipment    sql (read-only user)              row count > 0
2 is it relevant?    gatekeeper   classifier → prep (0.2–0.8)       held-out AUC; outage → prep
3 tag items          prep         <small-model-id>                  schema + replay passes bar (3f)
4 draft report       sous         <mid-model-id>                    checks.py (numbers = script, disclosure)
5 escalation         head chef    <strong-model-id>                 only when 4 fails twice; ceiling 10%/week
Tiers resolved <date> from <host list | models endpoint | pricing page>: prep=<id> sous=<id> head=<id> consultant=<id|none>
Pinned: <steps on fixed IDs>   Aliased: <steps on aliases>
Critic: <cross-family | same-family review | self-review>
Budget: <per-run cap> / <per-day cap>   Kill switch: <env/config line>   Drift sample: <rate, reviewer>
Evidence: <replay note path>   Rollback: <env/config line>   Escalation log: <path>
```
