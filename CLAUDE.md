# CLAUDE.md — Functional analysis project template (English variant)

> Copy this file as `CLAUDE.md` into the root of a new functional analysis project targeting an English-speaking/international audience.
> Update the structure section with the real project name.
> **Template version: 2.0** — see `CHANGELOG.md` in this repo (`sdd-template`) for the change history. This is a straight translation of the Spanish master (`CLAUDE.md`) in this same repo; the methodology is identical. If you improve this template on a concrete project, port the change back to both language variants and add the changelog entry.

---

## Project kickoff checklist

Before writing the first document:

- [ ] Repository created with `.gitignore` configured (excludes `.env`, `.env.local`, `*.pem`, `*.key`)
- [ ] `README.md` created with placeholder text (will be completed as the portfolio README when the project closes)
- [ ] `decisions.md` created with the empty entry template
- [ ] **Project profile chosen and logged in `decisions.md`** (see "Project profiles" below)
- [ ] `docs/00_glossary.md` created (can start empty)
- [ ] `docs/01_project_brief.md` created with the minimal structure
- [ ] `.env.example` created if the project has environment variables (example values, never real ones)
- [ ] `LICENSE` created (MIT for code; CC BY 4.0 if the main deliverable is documentation)
- [ ] Repository configured on GitHub: one-line description, relevant topics (e.g. `business-analysis`, `portfolio`, `spec-driven-development`)
- [ ] First commit with only the empty structure — before writing content

---

## Project profiles

Not every portfolio project needs all ten phases. Before writing the Project Brief, choose a profile and log it in `decisions.md` as the project's first decision ("Profile chosen: [Full/Light] — reason").

### Full profile

For the portfolio's centerpiece project: a real business domain, multiple roles/actors, at least one flow with non-trivial business rules. Follows the ten phases documented in this file without cuts.

### Light profile

For support tools, internal utilities, or exploring a specific technique: scope limited to one or two roles, without complex business rules. Changes relative to the Full profile:

- **00 Glossary, 01 Brief:** no cuts — these are the documents everything else anchors to.
- **02 User Stories:** the INVEST review (equivalent to 02b) is done inline at the end of the same document, not in a separate file.
- **03 + 04 + 05 merged** into a single `03_requirements.md`: business rules, functional requirements in EARS, non-functional requirements, and a requirements table with a MoSCoW column and verification method (RTM and MoSCoW summary combined). The MoSCoW warning signals are kept (no >80% Must, at least one justified Won't).
- **06 Data model:** skipped as a file if the domain has a single simple entity; if there are ≥2 related entities or one entity with its own lifecycle, document it the same as in the Full profile.
- **07 Flow map:** skipped as a file if all flows are linear (no decision points with diverging branches); in that case steps are documented as acceptance criteria of the user stories. If any flow branches, document it the same as in the Full profile.
- **08 Interface specification:** skipped if the project has no UI or a single trivial view (e.g. a single-action CLI).
- **09 Verification plan:** no cuts to discipline, but it can be a shorter table if there are few Must requirements.

General rule: merging or skipping a document is a decision, not a silent omission — log it in `decisions.md` before writing the Brief. If mid-project the scope turns out to be larger than expected for a Light profile, log the profile change as a new decision and resume the separate phases from that point.

---

## Methodology

This project follows **Spec Driven Development**. No phase advances until the previous phase's artifacts are completed, reviewed, and marked as baseline.

### Lifecycle of each document

```
draft → review → baseline
```

- **draft:** first complete write-up.
- **review:** re-read with stakeholder eyes — identify inconsistencies, gaps, and ambiguities before advancing.
- **baseline:** approved by the user. The header's `Status:` field changes to `baseline [date]`. Only from baseline does the project move to the next phase.

The `Status:` field in each document's header reflects which of the three states it is in. Example: `Status: baseline 2026-07-01`.

### Phase sequence (Full profile; see Light profile variants above)

```
Project Brief          ←── includes context diagram and privacy consideration
     │
     ├── Glossary      ←── living document; starts here and grows with each phase
     ↓
User Stories
     └──→ INVEST review (02b)  ←── closes the phase; no story advances without passing it
     ↓
BRD / PRD               ←── no flows section; may include a prose overview
     ↓
Requirements Traceability Matrix  ←── a real RTM; includes MoSCoW column and verification method for FR and NFR
     ↓
MoSCoW summary          ←── 1 page; closes the loop: cleans Won't/Could out of downstream artifacts
     ↓
Data model              ←── Must + Should entities only
     ↓
Flow map                ←── single source of authority on flows
     ↓
Interface specification
     ↓
Verification plan       ←── closes the loop between requirements and tests
     ↓
── Cross-document consistency ──   ←── cross-check before writing code
     ↓
Code
     ↓
Spec-vs-implementation review  ←── compiles gaps from development + detects new gaps
     ↓
Portfolio README        ←── orients the external evaluator; last step
```

### Cross-document consistency

Before writing code, verify these three traces:

1. Does every domain term introduced in any document appear in the glossary?
2. Is every FR in the RTM covered by at least one view in the interface specification?
3. Is every flow in the flow map traceable to at least one FR in the RTM?

If any trace fails, fix the affected documents before continuing.

### Gap during implementation

When, during coding, a requirement turns out to be wrong, impossible, or different from what was specified:

1. **Classify the gap:**
   - (a) Implement differently — the spec was wrong; correct it and implement the right version.
   - (b) Defer — out of scope for this version; move it to Should or log it as debt.
   - (c) Cancel — the requirement doesn't have enough value to implement.
2. **Update the affected spec document** — before implementing the change.
3. **Log it in `decisions.md`** with a title starting "Gap detected: [description]", the classification, and the reason.
4. **Update the RTM** if the change affects the test case or verification method.
5. Continue implementation.

Never implement a change without having updated the spec first. Code follows the documents, not the other way around.

The closing spec-vs-implementation review compiles all the gaps already logged in `decisions.md` during development, plus any gap not detected until that point.

---

## Standards by document

---

### 00 — Glossary

Living document. Opens with the Project Brief and is updated at every phase.

**Standard entry format:**

```
**[Term]** — [Full definition in one or two sentences].
Related to: [term1], [term2].
Introduced in: [name of the document where it first appears].
Updated: [date if the definition evolved from a previous version].
```

**What it must contain:**
- Every project-specific domain term: states, roles, process concepts, entity names.
- Any term an external reader could misunderstand.

**Rules:**
- No new domain term is introduced in a document without adding it to the glossary in the same session.
- Before baselining any document, Claude verifies that every domain term it introduces is in the glossary.
- If a term's definition evolves, update the entry and add the update date. Never overwrite silently.

---

### 01 — Project Brief

**Minimal structure:**

1. **Problem** — what happens today and why it's a problem. Include at least one data point or estimate that quantifies the impact ("the process takes X minutes", "it happens with frequency Y"). If no data is available, document it as an assumption.
2. **Proposed solution** — one line.
3. **Context diagram** — a simple diagram (can be text) showing: the system, its users, and its external dependencies (email services, databases, third-party systems). Contextualizes the system for any new reader.
4. **Stakeholders** — everyone affected, not just direct users. *(Stakeholders include direct users; the Users table only breaks out those who interact with the system.)* Technique: for each project goal, ask "who benefits?" and "who is negatively affected?". Document in a table: stakeholder / relationship to the project / main interest.
5. **Users** — table: role / main goal. Only those who interact directly with the system.
6. **Project goals** — measurable.
7. **Scope** — in / out, explicit. If there's ambiguity, resolve it here or log it as a pending decision.
8. **Constraints and assumptions** — check they cover the relevant categories: time, budget, technical, organizational, regulatory (for constraints); users, data, infrastructure, process (for assumptions). If any category is empty, justify why it doesn't apply.
9. **Privacy consideration** — if the system handles personal data (names, emails, identifiers): what data is stored? for how long? who has access? what happens to it when the system closes or resets?
10. **Success criteria** — must be SMART: specific, measurable, and time-bound. "Users will be able to use the app" is not a valid success criterion. **These criteria are revisited literally at project close (see "Portfolio README") to confront them with the actual outcome — they are not filed away once written.**
11. **Time horizon** — even if approximate.
12. **Pending decisions** — living list. Every pending decision must be resolved before the document it affects reaches baseline.

---

### 02 — User Stories

**Format:** As a [role], I want [action], so that [goal].
**Acceptance criteria:** Given / When / Then.

**Organization:** group by functional domain (access, user management, main process, administration). Not by implementation order.

**Definition of Done** (define at the start of the document and don't change it):
- Correct format (As a / I want / so that).
- At least one acceptance criterion for the happy path and one for the alternative or error path.
- INVEST review (02b, or inline in the Light profile) passed with no criterion in 🔴.
- All dependencies explicitly documented.

A story isn't considered closed until the INVEST review has passed it.

**Quality standards:**
- Sequential, clean IDs. If a story is removed during review, leave a note line: `~~US-08~~ — removed; content absorbed into US-07a`.
- Never merge two stories to avoid a dependency. The dependency is documented, not hidden.
- "System stories" ("As the system...") are not valid: they are system behaviors; they go as acceptance criteria of another story or as business rules in the BRD.
- Non-functional constraints specific to a story (response time, size limit) go in its acceptance criteria. System-wide constraints go in the BRD.

---

### 02b — INVEST review

In the Light profile, this review goes at the end of the same User Stories document instead of a separate file; the criteria and thresholds are the same.

**Scale:** 🟢 meets · 🟡 meets with caveats · 🔴 doesn't meet

**Thresholds:**
- Any 🔴 criterion blocks baseline. The story needs a fix before advancing.
- Three or more 🟡 criteria in one story signal a design problem, even if none is 🔴.
- I🟡 for "has no value without another story" is not acceptable without explicit justification: either merge with a documented reason, or split and document the dependency.
- For accepted 🟡s: document the reason and mitigation. Never accept a 🟡 silently.

**Warning sign:** if the full review produces no 🟡 or 🔴 at all, the review wasn't critical enough. Re-read every story asking "what could go wrong implementing this?". If, after the second read, every criterion is still 🟢, Claude must explicitly argue why each criterion is green before closing the review. A review with zero 🟡 requires justification, not just the result.

---

### 03 — BRD / PRD

In the Light profile, this document merges with 04 and 05 into `03_requirements.md` (see "Project profiles"); the content structure is the same, adding the requirements table with a MoSCoW column and verification method.

**Minimal structure:**
1. Context and reference
2. Process overview *(optional — two paragraphs of prose; no numbered steps; oriented to a business reader; not the source of authority on flows)*
3. Actors and permissions (one matrix per functional domain, not one mega-matrix)
4. Business rules (BR-xx)
5. Functional requirements in EARS format (FR-xx)
6. Non-functional requirements (NFR-xx)
7. Edge cases and exceptions

**Quality standards:**
- Sequential IDs for business rules and requirements. If one is removed: `~~BR-05~~ *(removed — see note in BR-12)*`.
- Business rules that result from a decision must reference `decisions.md` with the date of the corresponding entry. If a rule's reason isn't obvious, the reference to `decisions.md` is mandatory.
- The BRD has no detailed flows section. The "Process overview" is contextual prose, never numbered steps with decisions.
- No empty column filled with "—". If a column adds no value, remove it from the document.
- Before baselining: verify that all assumptions from the Project Brief still hold. If any changed during analysis: update the Project Brief and log the change in `decisions.md`.

**EARS format** for functional requirements:

| Pattern | Structure |
|--------|------------|
| Ubiquitous | *"The system shall..."* |
| Event-driven | *"When X, the system shall..."* |
| State-driven | *"While X, the system shall..."* |
| Optional | *"Where X, the system shall..."* |
| Unwanted behavior | *"If X, then the system shall..."* |

---

### 04 — Requirements Traceability Matrix (RTM)

In the Light profile, this matrix lives inside `03_requirements.md` (see "Project profiles").

This document is a real **Requirements Traceability Matrix**: it lets you follow a requirement from its origin (US) to its verification (test case).

**Mandatory columns:**

| ID | Description | Category | Source US | Related BR | MoSCoW priority | Verification method | Test case |
|----|-------------|-----------|--------------|----------------|------------------|------------------------|----------------|

**Rules:**
- **Verification method:** mandatory for every requirement, both FR and NFR. For FR: reference to the test or scenario ("E2E: test_create_group"). For NFR: tool and quantitative criterion ("axe DevTools — 0 level AA violations").
- FRs with no source US (system behaviors detected during analysis) are marked "—" in that column. At the bottom of the document, add a note explaining these requirements are derived from analysis, not from a user story.
- If a requirement changes during implementation, update the whole row; never leave stale data.

---

### 05 — MoSCoW

In the Light profile, this distribution lives as a column inside `03_requirements.md` (see "Project profiles"); the warning signals still apply the same way.

The MoSCoW document **is not a document that repeats every requirement**. It's a one-page document with:
1. Distribution table (Must / Should / Could / Won't by category).
2. Defense of the most important prioritization decisions: why X is Must and not Should, why Y is Won't.
3. List of Should/Could items with their manual workaround (what the user would do if this feature didn't exist).
4. List of Won't Have items with a note on whether it could be reconsidered in a future version.

Each requirement's priority lives as a **column in the RTM (04)**. The MoSCoW document references the RTM.

**Warning signs:**
- If more than 80% of requirements are Must Have: the scope isn't tuned to an MVP. Start from the minimal flow that solves the problem; classify everything outside it as Should/Could; argue only what has no viable alternative back into Must.
- If there is no Won't Have at all: the analyst hasn't demonstrated the ability to say no. Won't Have is not "what we didn't have time for" — it's "what was evaluated and explicitly ruled out for this version". Every professional spec has at least two or three.

**Closing the loop after MoSCoW:**
- Before moving to the data model and flow map: verify no downstream artifact includes Won't or Could items.
- The data model only models Must + Should entities and attributes.
- The flow map doesn't include flows for Won't or Could items.

---

### 06 — Conceptual data model

In the Light profile, this is skipped as a file if the domain has a single simple entity (see "Project profiles").

**Quality standards:**
- Only Must + Should requirement entities and attributes are modeled. Won't/Could items don't appear in the model.
- Exception: cross-cutting infrastructure entities (authentication, global configuration) that are needed regardless of feature priority. Label them "infrastructure" in the summary table.
- State transition diagram for every entity with its own lifecycle.
- Relationship table with cardinality and a note on the relevant business constraints.
- Entity summary table with MoSCoW priority.
- Attributes must include their validation constraints when not obvious (unique, not null, range, format).

---

### 07 — Flow map

In the Light profile, this is skipped as a file if all flows are linear (see "Project profiles"); in that case steps are documented as acceptance criteria of the user stories.

This document is the **single source of authority on flows**. If the BRD has a flows section, this document supersedes it.

**Structure per flow:**

```
## Flow N — [Name]

**Actors:** [list]
**Trigger:** [what action or event starts this flow]
**Precondition:** [system state before it starts]
**Postcondition:** [system state when it succeeds]
```

**Annotation legend:**
- `[D]` — decision point with branches → Yes / → No
- `[E]` — error or blocking path: the flow **does not reach the postcondition** → ends at `END ✗`
- `[A]` — alternative flow: reaches the **same postcondition** via a different path → ends at `END ✓`
- `[S]` — a Should-feature step
- `END ✓` — postcondition reached
- `END ✗` — flow ended without completing

**Rules:**
- References to FR and BR at every relevant step.
- Could/Won't items don't appear in flows.
- When one flow triggers another, reference it explicitly: `*(See Flow N.)*`.

---

### 08 — Interface specification

In the Light profile, this is skipped if the project has no UI or a single trivial view (see "Project profiles").

**Structure:**
- Navigation map at the start.
- Per view: elements (table), states, actions.
- Each view references the FRs it satisfies.

**States that must be covered systematically in every view:**

| State | Description |
|--------|-------------|
| Empty state | What the user sees when the list or section has no data |
| Loading state | Visual indicator while an async response is pending |
| Error state | What happens when the operation fails |
| Normal state | View with data |

**If there's an NFR for responsive interface:** for each view, indicate which elements adapt, collapse, or reorganize on small screens.

**Destructive or irreversible actions:** any action that can't be undone (delete, reset, confirm) must have its confirmation dialog specified: what it says, what options it offers.

---

### 09 — Verification plan

The document that closes the loop between requirements and tests.

**Structure:**

| Requirement ID | FR/NFR type | Description | Test type | Tool / Environment | Success criterion | Result |
|---|---|---|---|---|---|---|

**Test types:**
- **Inspection** — manual review of code, document, or configuration.
- **Demonstration** — run the case in the system and show the expected result.
- **Test** — automated test (unit, integration, or E2E).
- **Analysis** — evaluation with a specialized tool (axe, Lighthouse, W3C Validator).

**Rules:**
- Covers every Must Have (FR and NFR). Should Haves appear with a note that they're optional in this version.
- For NFR: a quantitative criterion is mandatory. Not "meets WCAG" but "0 level AA violations in axe DevTools across views V-01 to V-10".
- The "Result" column is filled in during verification, not before. A blank result is an unverified requirement; document the reason if omitted intentionally.

---

## Spec-vs-implementation review

When closing out the code, before considering the project done:

1. Collect all gaps logged in `decisions.md` as "Gap detected" during development.
2. Walk through every Must Have in the RTM and verify each is implemented and verified.
3. For new gaps detected in this step: classify, document in `decisions.md`, and update the verification plan.

---

## Portfolio README

Last step of the project. One page that orients the external evaluator:

- What problem the project solves and why the documentation is worth reading.
- List of documents in recommended reading order, with a one-line description each.
- Two or three key decisions that demonstrate the analytical reasoning behind the project.
- Project status: what was delivered, what was deferred, and why.
- **Results against the Brief's success criteria.** Mandatory table that revisits every SMART criterion defined in `01_project_brief.md` literally and confronts it with the actual result:

  | Success criterion (Brief) | Outcome | Met? |
  |---|---|---|

  If a criterion wasn't met, explain why and what it would take to meet it — don't drop the row or reword the criterion after the fact to make it "fit" the result.

---

## GitHub and public portfolio

### Commits as a portfolio artifact

The commit history is visible on GitHub and must tell the project's story: documentation phase by phase first, then implementation. An evaluator looking at the history should be able to see that the SDD process was actually followed.

**Commit message convention:**

```
docs: [phase] — [brief description]     ←── documentation commits
feat: [brief description]               ←── new functionality
fix: [brief description]                ←── bug fix
test: [brief description]               ←── tests
chore: [brief description]              ←── maintenance tasks (deps, config)
```

Examples:
- `docs: project brief — baseline`
- `docs: user stories — INVEST review completed`
- `docs: brd/prd — baseline`
- `feat: authentication — login and first access`

**Rules:**
- One commit per documentation phase when it reaches baseline, not one per edit.
- Messages describe the outcome ("baseline", "review completed"), not the action ("edit", "add", "change").
- Never mix documentation commits and code commits in the same push.

### README.md as the GitHub landing page

The README.md is the first thing anyone sees when they land on the repository — it renders directly on GitHub's main page. It must be designed for that format, not as a plain text document.

**Recommended structure for the final README:**

```markdown
# [Project name]

[One line describing what it does and for whom.]

## The problem

[2-3 sentences: what happened before, why it was a problem, what solves it.]

## Documentation

| Document | Description |
|-----------|-------------|
| [Project Brief](docs/01_project_brief.md) | Problem, scope and goals |
| [User Stories](docs/02_user_stories.md) | User stories with acceptance criteria |
| ... | ... |

## Results against success criteria

[Table — see "Portfolio README" section above.]

## Tech stack *(if applicable)*

[List of main technologies.]

## Project status

[What's delivered, what was deferred and why.]
```

**Rules:**
- If the project has a visual interface, include at least one screenshot (`![description](path/image.png)`).
- If the project is deployed, include the link.
- Links to documents must use relative paths so they work both on GitHub and locally.
- The placeholder README (during development) can be just the title and one line. Never leave it empty: GitHub shows an empty README as a sign of abandonment.

### License

A public repository without a `LICENSE` is technically "all rights reserved", which legally prevents others from using or referencing the work.

- For projects where the code is the main deliverable: **MIT**.
- For projects where documentation is the main deliverable: **CC BY 4.0** (Creative Commons Attribution).
- For mixed projects: MIT for the code, CC BY 4.0 for the documentation — specify it in the LICENSE itself or in the README.

---

## Decision log (`decisions.md`)

Captures **every** relevant project decision, not just analysis ones:

- The project profile chosen (Full/Light) and its reason — the project's first entry.
- Analysis decisions: scope, roles, business rules, mechanisms.
- Design decisions with functional consequences: authentication architecture, session model, integrations, tech stack.
- Gaps detected during implementation or in the final review. These entries' titles must start with **"Gap detected:"** so they're easy to spot in the log.

Gaps are documented **when they're detected**, not only at the end of the project.

**Entry format:**

```
### [DATE] — [BRIEF TITLE]

- **Status:** active
- **Decision:** what was decided
- **Context:** why this needed to be decided
- **Alternatives discarded:** what was considered and why it wasn't chosen
- **Consequences:** what this implies going forward
```

**Reverting a decision:** create a new entry that references the original and mark the original as superseded:

```
### [ORIGINAL DATE] — [TITLE]
- **Status:** ~~active~~ superseded — see [NEW DATE]
...

### [NEW DATE] — Revision of [ORIGINAL TITLE]
- **Status:** active
- **Decision:** the decision from [ORIGINAL DATE] is reverted. [New decision].
- **Context:** [what changed to make the previous decision no longer valid]
...
```

---

## Backlog (`BACKLOG.md`)

Captures every idea, unresolved question, proposed feature, or future fix the user mentions in conversation that won't be addressed right away — it shouldn't just be left in the conversation history.

- Logged in the same turn it's mentioned, using the format already established in `BACKLOG.md` (Origin / Description / Related to / Open questions).
- The user doesn't need to explicitly say "this goes to the backlog": if something is raised as a future idea, an unresolved question, or a possible fix that's postponed, log it anyway.
- If the question is resolved in the same conversation (even if it was mentioned as a possible backlog item), document it as resolved too — see existing examples in `BACKLOG.md` — instead of omitting it.
- A backlog item never enters a formal phase (User Stories, BRD, RTM...) without first passing through an explicit user decision, as `BACKLOG.md`'s header states.

---

## Security

### Credentials and secrets

- Never embed credentials, tokens, or secrets in URLs, code, or versioned configuration files.
- Any operation involving authentication or credentials must be run by the user directly in their terminal.
- Before running any command with security implications: explain what it will do and wait for explicit confirmation.

### Environment variables

- Every environment variable the project needs is documented in `.env.example` with example values or descriptions (never real values).
- `.env.example` is versioned. `.env`, `.env.local`, and any file with real values are never versioned.

### Public repository

If the repository is public (the usual case for a portfolio), everything pushed is permanently visible. Additional implications:

- Seed and sample data must be **entirely fictitious**: made-up names, emails like `student1@example.com`, no real data from people even if it's test data.
- Analysis documents must not contain real data about users, institutions, or third-party systems even if partially anonymized.
- Before every push, mentally review whether anything in new or modified files shouldn't be public.

### Before the first commit

Verify that `.gitignore` excludes:
- `.env`, `.env.local`, `.env.*.local`
- `*.pem`, `*.key`, `*.p12`
- Development environment credential/secret directories

If the project adds new dependencies: check for known vulnerabilities before committing (`npm audit` or equivalent).

---

## Working norms

- The user thinks and decides. Claude guides, proposes options, and warns of consequences, but does not decide.
- For any decision affecting scope, requirements, architecture, or user experience: present options with pros/cons and wait for the user to choose.
- Claude drafts complete proposals; the user reviews, corrects, and approves.
- Before updating `decisions.md`, the user must have argued the decision in conversation.
- If the user wants to skip a phase: flag it and redirect.
- If the user wants to mark a document as baseline without having gone through review: flag it.
- If something in the spec doesn't make sense or looks wrong: say so with arguments before executing.

---

## User Stories review

Flow: drafting → INVEST review → correction → baseline.

When the user presents the user stories, load `docs/02b_user_stories_invest.md` (or the inline equivalent section in the Light profile) and fill in the review before any other step.

---

## Documentation structure (Full profile)

```
[project-name]/
├── CLAUDE.md
├── README.md               ←── placeholder text until closing; completed as the portfolio README
├── decisions.md
└── docs/
    ├── 00_glossary.md      ←── living from the start
    ├── 01_project_brief.md ←── includes context diagram
    ├── 02_user_stories.md
    ├── 02b_user_stories_invest.md
    ├── 03_brd_prd.md
    ├── 04_requirements_matrix.md
    ├── 05_moscow.md
    ├── 06_data_model.md
    ├── 07_flow_map.md
    ├── 08_interface_spec.md
    └── 09_verification_plan.md
```

## Documentation structure (Light profile)

```
[project-name]/
├── CLAUDE.md
├── README.md
├── decisions.md
└── docs/
    ├── 00_glossary.md
    ├── 01_project_brief.md
    ├── 02_user_stories.md           ←── includes inline INVEST review
    ├── 03_requirements.md           ←── BRD/PRD + RTM + MoSCoW merged
    ├── 06_data_model.md             ←── only if applicable (see "Project profiles")
    ├── 07_flow_map.md               ←── only if applicable
    ├── 08_interface_spec.md         ←── only if applicable
    └── 09_verification_plan.md
```
