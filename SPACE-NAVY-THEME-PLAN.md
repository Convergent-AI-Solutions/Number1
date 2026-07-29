# Re-skin: "navy first mate" → "space navy / Number One"

## Context

This repo is Firstmate — an agent-orchestration "distro" whose entire personality is a
nautical **navy / first mate / captain / crew** theme. The captain wants that theme
re-skinned to a **space navy** voice modeled on a Next-Generation-style starship bridge,
while **deliberately avoiding one-to-one copyrighted references** (no ship names, character
names, or signature catchphrases — only generic naval/space rank words, which predate and
are not owned by any franchise).

Hard requirement from the captain: **functionality does not change — only the theme.**
The survey found the theme words are deeply entangled with functionality: `captain`, `crew`,
`secondmate`, `fleet`, `bearings`, `ship`, `scout` appear both as flavor prose **and** as
load-bearing identifiers (script names, env vars, file paths, task-kind values, skill
directory names, machine-protocol tokens, test assertions). So the whole job is a
**position-aware prose re-skin**: rename the flavor, never touch an identifier.

Intended outcome: reading `AGENTS.md`, `README.md`, docs, and skill prose feels like a
starship bridge with the assistant as "Number One"; every script, test, env var, path, and
task kind behaves exactly as before; the full test suite + lint + doc-audience check pass.

## Decided vocabulary map (captain-approved)

| Current (prose) | Space-navy (prose) | Notes |
|---|---|---|
| product brand `firstmate` / `Firstmate` (visible titles/prose) | **Number One** | identifiers `fm`/`firstmate` stay everywhere in code |
| "the first mate" / "You are the first mate." (persona) | **Number One**, role **first officer** → "You are Number One, the ship's first officer." | |
| `captain` (user's call-name & address) | **captain** — UNCHANGED | fits a starship captain; also test-guarded |
| `crewmate` / `crew` (worker, prose) | **crew** / **crew member** | |
| `second mate` / `secondmate` (persistent worker, prose) | **second officer** | identifier `secondmate` stays in code |
| `scout` (prose) | **scout** — kept | also a functional `kind=scout` |
| `fleet` (prose) | **fleet** — kept | fits space navy; also an identifier in places |
| `ship`/`ships` (verb/tagline) | **ship**/**deliver**/**starship** as fits | the task-kind `ship` stays in code |
| seasoning: `shipshape` | **all systems nominal** | see lockstep below |
| seasoning: `on deck` | **on station** / **on the bridge** | |
| seasoning: `aye`, `under way` | kept (both valid bridge/spaceflight terms) | |
| meta-word `nautical` (describes the theme itself) | **space navy** / **bridge** | AGENTS.md ~L10, ~L389 |

Flavor level: **richer bridge flavor** — lean into bridge idioms (on station, all systems
nominal, hailing, systems green) where the existing text already carried seasoning; keep it
out of commits/PRs/briefs and out of bad-news delivery, exactly as the current contract
already requires.

## The golden rule — NEVER rename these (functionality)

A theme word is **load-bearing** (leave byte-for-byte) whenever it appears as:
- a `bin/**` or `tests/**` filename stem (e.g. `fm-crew-state.sh`, `fm-fleet-snapshot.sh`,
  `fm-bearings-snapshot.sh`, `fm-secondmate-report.sh`) or the `fm-`/`fmx-` prefix itself;
- an env var — every `FM_*` / `FMX_*` (e.g. `FM_HOME`, `FM_SECONDMATE_SCOPE`, `FM_CAPTAIN_RE`,
  `FM_BEARINGS_*`, `FMX_PAIRING_TOKEN`) and the `FIRSTMATE_OP:` operational prefix;
- a path literal under `config/`, `data/`, `state/` (e.g. `config/crew-harness`,
  `config/secondmate-harness`, `config/crew-dispatch.json`, `data/captain.md`,
  `data/captain-shared.md`, `data/secondmates.md`);
- a `.meta` key or enum value — especially `kind=ship`, `kind=scout`, `kind=secondmate`;
  spawn modes `crew`/`secondmate`; harness names; JSON tokens `fm-bearings.v1`,
  `fm-fleet-snapshot.v1`, `captain_decision`, `captain-hold`, hold `--kind captain`;
- a status-protocol verb matched by regex (`done:`, `needs-decision:`, `blocked:`, `failed:`,
  `working:`, `paused:`, `PR ready`, `checks green`, `ready in branch`, `merged`);
- a skill directory name / slash command (`ahoy`, `bearings`, `afk`, `stow`,
  `secondmate-provisioning`, `stuck-crewmate-recovery`, `firstmate-orca`,
  `firstmate-codexapp`, `firstmate-coding-guidelines`, `updatefirstmate`, etc.) or its
  frontmatter `name:`;
- a diagnostic prefix consumed by another component (`CREW_DISPATCH:`, `FLEET_SYNC:`,
  `SECONDMATE_SYNC:`, `SECONDMATE_LIVENESS:`, `NUDGE_SECONDMATES:`, `BOOTSTRAP_INFO:`);
- any external URL/handle (`github.com/kunchenguid/firstmate`, treehouse link, x.com/discord
  badges).

Re-skin only **free prose**: narrative sentences, comments, echoed human-readable
help/error strings — and only after confirming the exact string isn't a test assertion or
embedded identifier (see lockstep section).

## File-by-file scope

**`AGENTS.md`** (master persona contract — heaviest prose surface; `CLAUDE.md` is a symlink to it):
- Title `# Firstmate` → `# Number One`.
- Preamble identity (L3–5): "You are the first mate." → "You are Number One, the ship's first officer."; keep "The user is the captain." and "This file is your entire job description."
- Mandatory-address rule (L7–9): keep "captain" verbatim (unchanged + test-guarded); refresh any nautical example wording to bridge wording.
- Seasoning menu (L10): rewrite the flavor word list from `"aye", "on deck", "shipshape", "under way", "ahoy"` to bridge seasoning (e.g. `"aye", "on station", "all systems nominal", "under way", "systems green"`); keep the constraint that seasoning never appears in commits/briefs/PRs and is dropped for bad news. Change the meta-word "nautical" → "space navy".
- Body role words used as narrative (not identifiers): "first mate"→"Number One"/"first officer", "crewmate"/"crew"→"crew"/"crew member", "second mate"→"second officer". Leave every `config/…`, `data/…`, `bin/…`, `FM_*`, `kind=…`, skill-name token exactly as written.
- §9 escalation etiquette: **lockstep** — see below (`Captain, shipshape.` and the "house vocabulary" line are test-asserted).

**`README.md`** (public overview — keep it concise, pointers-only per CONTRIBUTING):
- `<h1 align="center">firstmate</h1>` → `Number One`; tagline "Talk to one agent. Ship with a crew." → a bridge tagline (e.g. "Talk to Number One. Command a crew."); banner `alt` text updated.
- "What it is" / "Features" / "How It Works" persona prose: "the first mate"→"Number One", "crewmate"/"crew"→"crew", "second mates"/"secondmates" (prose)→"second officers", diagram labels `you (the captain)` keep, `crewmate`→`crew`. Keep every code-ish token (`FM_HOME`, `backend=orca`, `fleet-sync`, `/afk`, `/ahoy`, `/bearings`, `/stow`, `/updatefirstmate`, `2ndmate-<id>`) unchanged.
- Example prompt `> ahoy! …` (L117): re-skin the greeting word (the `/ahoy` skill stays); example output `PR ready for review, captain:` keep "captain".

**`CONTRIBUTING.md`** (light): L36 "firstmate orchestrator agent" and L40 "captain's fleet" persona sentences re-skinned; leave all script/env/path references and the symlink assertions untouched.

**`docs/**.md`** (33 files — mostly functional identifiers): re-skin only persona/narrative mentions of "first mate", "crewmate"/"crew", "second mate". `architecture.md` carries the most re-skinnable narrative. `configuration.md` and `scripts.md` are almost pure identifier reference — touch prose only, and treat them as highest-risk-for-accidental-identifier-edit. Run `bin/fm-doc-audience-check.sh` after.

**`.agents/skills/*/SKILL.md`** (18 internal skills): re-skin prose bodies. Skill **directory names**, frontmatter `name:`, and any **contract lines the tests assert** stay byte-exact. Frontmatter `description:` doubles as trigger text — re-skin persona words there but preserve any test-asserted substrings. Highest-care skills: `bearings` (chat-response contract), `ahoy`, `fmx-respond`, `decision-hold-lifecycle`.

**`skills/stow/SKILL.md`** (public installer-facing): **LEAVE UNCHANGED.** It is intentionally persona-free/theme-free by design (says "user", no nautical vocabulary); the internal `.agents/skills/stow` is the themed one.

**`bin/**`**: re-skin only echoed prose strings and narrative comments (e.g. teardown operator guidance in `fm-teardown.sh`, guard message in `fm-guard.sh`, rebase hint in `fm-merge-local.sh`). NEVER touch identifiers, paths, protocol verbs, or the `Captain, shipshape.` string without its test. Grep `tests/` for any echoed string before editing it.

## Lockstep test surfaces (change source + test + cross-refs together)

1. **`Captain, shipshape.`** exact reply (AGENTS.md §9 ~L422) → **`Captain, all systems nominal.`**
   - Must update in lockstep: `tests/fm-captain-translation-contract.test.sh` (asserts the exact string, ~L109), `tests/fm-x-mode.test.sh`, and every prose reference to "shipshape"/that reply in `bin/fm-decision-hold.sh`, `bin/fm-fleet-snapshot.sh`, `bin/fm-bearings-snapshot.sh`, `.agents/skills/bearings/SKILL.md`, `.agents/skills/decision-hold-lifecycle/SKILL.md`, `docs/decision-hold-lifecycle.md`. Grep the whole repo for `shipshape` first and change every hit consistently.
2. **"Scout and second mate are accepted Firstmate nautical house vocabulary and do not need translation"** (AGENTS.md §9 ~L389) → e.g. "Scout and second officer are accepted Number One house vocabulary and do not need translation".
   - Lockstep: `tests/fm-captain-translation-contract.test.sh` asserts this string verbatim (L45). Update assertion to match.
3. **Bearings chat-response headings** — "Captain's Call" / "Recently Landed" / "Underway" / "Charted Next" (and the removed "At Anchor"): **recommend keeping as-is.** All four already read correctly in a space-navy frame (star charts, a ship landing, under way, the captain's call), and they carry a fragile 3-way lockstep (`.agents/skills/bearings/SKILL.md` + `tests/fm-bearings-snapshot.test.sh` + `.agents/skills/decision-hold-lifecycle/SKILL.md` + an AGENTS.md mention). Skipping them removes risk for zero thematic loss.
4. General rule for the implementer: **before editing any prose string that contains a theme word, `grep tests/ -F "<string>"`.** If a test asserts it, edit test + source in the same commit. Other tests that track skill/contract prose (`fm-session-start.test.sh`, `fm-bootstrap.test.sh`, `fm-quota-array-dispatch.test.sh`, `fm-no-mistakes-ownership.test.sh`, `fm-decision-hold-lifecycle.test.sh`, `fm-secondmate-*`) mostly assert identifier/enum forms (`captain` kept, `kind=secondmate`, `CREW_DISPATCH:`) that we are NOT changing — verify, don't assume.

## Out of scope / notes

- `assets/banner.png` is themed art ("firstmate — talk to one agent, ship with a crew"). The re-skin can update the README `<img alt="...">` text and surrounding prose, but the image file itself must be regenerated by the captain in a separate image-generation session and dropped in at `assets/banner.png` (same path, so no README `src` change is needed). Until then the banner shows old branding — acceptable short-term.

  **Image-generation prompt for the captain to paste into an image session (primary — full banner):**

  > Wide horizontal banner image, approximately 1280×400 px (about 3:1), for an open-source developer tool named **"Number One"**. Theme: a sleek, modern spacefaring naval bridge — clean, optimistic science fiction inspired by late-80s/90s starship-bridge aesthetics but **entirely original** (do NOT depict any existing franchise's ships, logos, insignia, uniforms, interface/LCARS-style panels, or recognizable characters). Scene: a calm, confident first officer figure (gender-neutral, shown from behind or in silhouette) standing at a central command station, overseeing a semicircular bridge; in front of them several glowing console stations, each with a crew member working in parallel, are subtly linked to the central officer by thin light-lines — visually conveying "one officer coordinating many." A deep-space viewport with stars and a distant planet fills the background. Palette: "space navy" — deep navy blue and near-black, with cool cyan/teal and soft amber console glows; clean, high-contrast, professional, uncluttered, generous negative space. Include the wordmark **"Number One"** in a clean modern sans-serif, plus a smaller tagline **"Talk to one officer. Command a crew."**, crisp and legible. Style: polished cinematic digital illustration / concept art with soft lighting and subtle depth of field. No real trademarks, brand logos, or copyrighted UI styling.

  **Alternative prompt (minimal wordmark, if a cleaner logo-style banner is preferred):**

  > Minimalist wide banner, ~1280×400 px, dark "space navy" (deep navy-to-black gradient) background with a faint starfield and one subtle cyan orbital arc. Centered wordmark **"Number One"** in a clean modern geometric sans-serif with a soft cyan glow, and a small tagline beneath: **"Talk to one officer. Command a crew."** Lots of negative space, high legibility, professional open-source project banner. Entirely original — no existing sci-fi franchise imagery, logos, or trademarks.
- The `@myfirstmate` example handle in README X-mode prose is illustrative; may be updated to a space-y example handle, but it's not load-bearing.
- No file/script/skill **renames** anywhere — that would be a functionality change and is explicitly excluded.

## Execution note

Per `CONTRIBUTING.md`, changes to firstmate's tracked material must load the agent-only
**`firstmate-coding-guidelines`** skill first, follow one-sentence-per-line Markdown, keep
`README.md` concise, and ship through the repo's normal review path. Work happens on branch
`claude/space-navy-theme-redesign-5q5w0t`.

## Verification (end-to-end)

Run from the repo root after edits:
1. `bin/fm-lint.sh` — must pass (single owner of the shellcheck set; CI + gate both run it). Confirm the pinned shellcheck version with `bin/fm-lint.sh --required-version` first.
2. Syntax-check the shell surface: `while IFS= read -r s; do /bin/bash -n "$s" || exit; done < <(bin/fm-lint.sh --list-files)`.
3. `bin/fm-test-run.sh --all` — full behavior regression (this re-skin touches contract prose broadly, so a complete walk is warranted rather than `--changed`). Pay special attention to `fm-captain-translation-contract`, `fm-x-mode`, `fm-bearings-snapshot`, `fm-session-start`, `fm-bootstrap`.
4. `bin/fm-doc-audience-check.sh` — after the `docs/**` and skill edits.
5. Symlink integrity: `[ "$(readlink CLAUDE.md)" = "AGENTS.md" ]` and `[ "$(readlink .claude/skills)" = "../.agents/skills" ]`.
6. Sanity greps that identifiers survived: confirm `bin/`, `FM_*`, `config/`, `data/`, `state/`, `kind=`, and skill-dir names are unchanged (`git diff --stat` should show no renamed/added/removed files; diff should be prose-only). Confirm no stray "firstmate"→"Number One" edit landed inside a code identifier.
