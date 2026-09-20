# Decision log — household_os

Captures all relevant project decisions: analysis, design decisions with functional consequences, and gaps detected during implementation.

Entry format:

```
### [DATE] — [BRIEF TITLE]

- **Status:** active
- **Decision:** what was decided
- **Context:** why this needed to be decided
- **Alternatives discarded:** what was considered and why it wasn't chosen
- **Consequences:** what this implies going forward
```

---

### 2026-09-20 — Project profile: Full

- **Status:** active
- **Decision:** household_os follows the Full profile of the SDD template (`CLAUDE.md`), with all ten phases, no cuts.
- **Context:** at project kickoff, a choice had to be made between the Full and Light profile (template v2.0). household_os is a household-organization tool (shared calendars, events, and tasks) with several household roles, entities with their own lifecycle (tasks, recurring events), and flows with real decision points (assignment, notifications, what happens when a task isn't completed) — this fits the Full-profile criteria, not a narrowly scoped utility.
- **Alternatives discarded:** Light profile — discarded because it would have forced merging documents (BRD+RTM+MoSCoW) and skipping the data model/flows/interface in a project that, given its real scope, actually needs them.
- **Consequences:** the ten phases (00 to 09) are documented separately, following the Full profile's standard sequence.

---

### 2026-09-20 — Project language: English

- **Status:** active
- **Decision:** household_os is documented and coded in English — all `docs/`, `decisions.md`, `BACKLOG.md`, `README.md`, and code/commit content. File names follow the English document names (`00_glossary.md`, `01_project_brief.md`, ...), per the same precedent already set in `kintask` ("file names match the content's language").
- **Context:** unlike `kintask` and `the_refounders` (both in Spanish), this project targets an international/English-speaking job market. Most BA/PM job postings, the BABOK itself, and most recruiters reviewing a GitHub portfolio read English, not Spanish.
- **Alternatives discarded:** keeping Spanish for consistency with the other two portfolio projects — discarded because portfolio reach for the target market matters more than internal consistency across projects; a portfolio can credibly include pieces in more than one language when it's a deliberate choice, which this is.
- **Consequences:** the already-created placeholder files (`docs/00_glosario.md`, `docs/01_brief_proyecto.md`) were replaced by their English equivalents before any real content was written into them, so no translation debt is carried forward. Any future portfolio project targeting an international audience can start from the English template variant (`CLAUDE.en.md` in `sdd-template`) directly instead of repeating this decision.

---
