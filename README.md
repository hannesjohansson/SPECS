# Specs

A spec-driven operating model for product development in the agentic age.

For decades, the tools of structured product development lived on one side of a wall. Source control, config-as-code, CI pipelines, schema validation: engineers worked in these systems daily. Everyone else worked in documents, slides, spreadsheets, and project boards. Two worlds, connected by meetings, handoffs, and the slow decay of context between them.

That wall is gone. When a PM runs an agent, the agent reads files, writes structured YAML, commits to git, and validates its own output. The PM doesn't need to learn git. The agent handles the tooling. The barrier that kept non-engineers out of the repo collapsed because agents made it irrelevant.

If everyone can work in the same repo, then everything that matters to product development can live there: goals, hypotheses, specs, findings, evidence, retros, the planning itself. Source-controlled, diffable, reviewable, machine-readable. One connected system that evolves with the team.

Structural overhead used to be expensive. Writing structured specs, maintaining relationships between artifacts, keeping a release map current, logging findings, running validation. All of this cost human time that could have gone to building. Teams rightly minimized it. In an agentic world, that cost inverts. Agents draft specs, extract rules, generate checks, detect drift, reconcile status, flag stale claims. The maintenance work that made structured processes uneconomical is now close to zero. The coordination value of a single source of truth that all collaborators read and write is higher than it has ever been.

This is that source of truth. A spec-driven, hypothesis-led operating model where process, planning, evidence, and work all live in one place. A PM frames a goal, an engineer writes a rule, an agent generates code, a verifier produces a check, a researcher interprets a finding. They all read and write the same artifacts. No retranslation between roles. No private context. No lossy handoffs.

Specs govern intended product behavior. They are the authoritative source for what the team has agreed to build and why. Other authorities exist (legal constraints, platform conventions, runtime reality, incident response) and the system interacts with them without subsuming them. The source of truth changes first. Code follows.

---

## The loop

**Frame → discover → define → scope → commit → plan → implement → ship → measure → reflect.**

**Frame.** Narrative, architecture, goals set durable context.

**Discover.** Discoveries reduce uncertainty; findings record what was learned.

**Define.** Stories capture user needs. Goals define what to move.

**Scope.** A release file states the coordinated bet.

**Commit.** Specs define what will be built and why.

**Plan.** Work is broken into claimable items with dependencies. Agents can self-serve.

**Implement.** Agents claim work items, generate checks and code from accepted specs.

**Ship.** Code ships. Blueprints regenerate to reflect the new reality.

**Measure.** Findings record outcomes against spec and release hypotheses.

**Reflect.** The next release is scoped from results and retros.

Specs are bets at the feature level. Releases are bets at the team level. Findings measure outcomes. Goals are what you're moving over time. Discoveries reduce uncertainty before committing. Retros improve the system itself.

The release is the default moment the loop ticks. Continuous-shipping and infrastructure teams define equivalent learning windows — the point is that bets must be measured against evidence at explicit moments, not that shipments must be batched.

No phase lives outside the system. The loop runs entirely on artifacts the whole team shares.

---

## Five concepts

Five concepts carry most of the weight. Everything else is optional structure that emerges as work demands it.

**Spec** — The smallest authoritative coordination unit for an intended product change. Rules that code must match, plus a hypothesis about why those rules will produce a good outcome.

**Release** — A coordinated bet. A bundle of specs that ship together and test a shared hypothesis. The default unit where outcomes are measured.

**Finding** — A distilled piece of evidence. Validates intent, measures outcomes, or reveals new needs.

**Goal** — What the team is trying to move over time. The durable objective that releases aim at.

**Direction** — The framing material that all collaborators share: narrative, architecture, principles, discoveries, findings. Lives under `direction/`.

If a team understands these five, they can use the system. The rest of the vocabulary handles specific situations.

---

## Collaboration modes

The operating model serves three modes of collaboration simultaneously.

**Human to human.** Multiple people working from the same release file, story set, and spec format do not need to reconcile separate mental models. Alignment happens by reading, not by meetings. The artifact is the agreement.

**Human to agent.** A human frames intent and accepts outcomes. An agent reads the spec and does the volume work — drafting, generating, verifying, detecting drift. The interface between them is the spec itself, not natural language instructions that have to be reinterpreted each turn.

**Agent to agent.** Multiple agents can work in parallel on different specs without coordinating directly, because the specs themselves carry bounded context, stable references, and explicit authority. One agent extends a shared rule; another generates code against specs that reference it; a third runs drift detection across the result. They coordinate through the system, not through each other. The work manifest makes this concrete — agents claim items from a shared queue, declare dependencies, and signal completion through the same file layer that carries everything else.

Humans orchestrate, judge, prioritize, and accept. Agents are primary operational users of the system — they read it, write to it, and do most of the work inside it. Scripts verify mechanically.

---

## Principles

**Specs are the authoritative source for intended product behavior.** Rules describe what the team agreed to build. Code must match them. Unrecorded drift is an error. Recorded deviation is tracked debt.

**The commitment and the rationale are different interfaces.** Rules are authoritative and prescriptive. Hypothesis and rationale are predictive. Confirming or refuting a hypothesis does not change what the rules say — it informs what to do next.

**Every spec is a bet. Every release is a bigger bet.** Individual specs rarely move metrics alone. Releases are the honest unit for measurement.

**Two reasons to build anything.** A product spec addresses a user need, a business goal, or both. An architecture spec improves a system property. If a spec does neither, it should not exist.

**Discovery reduces uncertainty before committing.** When evidence is thin, run a discovery first. It produces a finding, not code.

**Direction is layered by rate of change.** Narrative changes quarterly. Architecture and blueprints change when things ship. Goals and stories evolve with understanding. Specs change weekly. Findings accumulate forever.

**Structure is extracted, not written upfront.** Duplication is the forcing function. When the same story, rule, or journey appears in multiple places, extract it.

**Humans curate, agents draft, scripts verify.** Humans are accountable for framing, judgment, and acceptance. Agents are primary operational users — they read the system, draft specs, extract rules, generate checks and code, detect drift. Scripts handle mechanical correctness.

**Change the source of truth first.** Never patch generated code silently. Update the spec, regenerate, verify. When deviation is unavoidable, record it. The spec stays authoritative.

**Bounded context is an interface.** Collaborators read what the spec links to, nothing more. The spec is the routing layer. The agents file is a routing table, not a manual.

**Parallelism is a design goal.** Multiple agents work safely in parallel because the system provides stable references, explicit authority, work manifests with explicit dependencies, and verification at the boundaries. The system is built for concurrent use, not sequential human-driven use with agents assisting.

**The system learns about itself.** Friction and insight are logged in retros. We review and act on the log. The operating model tightens over time.

---

## Non-goals

The system is designed for product development where specs govern intended behavior and evidence closes the loop. It is not:

- Required for throwaway prototypes with no intent to learn
- A replacement for legal, security, or compliance review
- A replacement for incident response or operational runbooks
- Suited to one-line fixes that don't reveal a gap in a spec's rules
- A forcing function for a heavyweight release train — continuous-shipping teams define equivalent learning windows
- An attempt to govern runtime behavior, platform constraints, or any authority outside product intent

A spec is warranted when the change is meaningful enough that its intent should be recorded, its outcome should be measured, or its rules will be referenced by future work. Below that threshold, use the issue tracker.

---

# Part 2 — Artifacts

## Vocabulary

### Direction — where are we

**Narrative** — What the product is, who it's for, why it exists. The durable story that outlives any feature. The narrative can be split into a short always-read `narrative.md` (the minimum every task should carry) and a triggered `narrative-extended.md` (strategic framing: direction trade-offs, positioning, acquirer thesis, why-we-are-not). The split keeps the always-read context small.

**Blueprint** — A derived view of the current user journey: frontstage actions, touchpoints, backstage processes, support systems. Generated by `graph` from shipped specs and their journey phase tags, so it stays current without manual maintenance. Post-traction, teams may choose to curate a richer version when the generated view isn't detailed enough for coordination, but the default is generated, not hand-written.

**Architecture** — Service boundaries, data model, technology choices, deployment topology, integration points. Describes system structure.

**Principles** — Design, engineering, and product standards. Describes how the team makes consistent choices.

The distinction between architecture, principles, and shared rules: architecture is structural, principles are dispositional, shared rules are concrete agreements that constrain multiple specs. When in doubt, a rule appearing verbatim in two specs is a shared rule; a rule that varies in expression but reflects a common value is a principle.

### Planning — where are we going

**Goal** — A business objective with measurable outcomes, structured as Goal / Signals / Metrics. Has optional kill criteria. Every release maps to existing goals; a release exploring new territory drafts the goal first and references it.

**Story** — A structured user need: "When [situation], I want to [motivation], so I can [outcome]." Lives inline in a spec until the same need appears in two or more specs, then extracted. A story may remain `validated` across multiple partial-address releases.

**Release** — A bundle of specs that ship together and test a shared hypothesis.

**Release map** — The connective tissue between releases and the user journey. Lists all releases and their states, connects each release's scope to the goals it aims at and the journey phases it affects. The place where you see how individual bets add up to a coherent product direction.

### Delivery — what are we building right now

**Spec** — The smallest authoritative coordination unit for an intended product change. One of three types:

**Product spec** — Rules plus a hypothesis that these rules will move a product metric. Requires story and goal connections.

**Architecture spec** — Rules plus a hypothesis that these rules will improve a system property. Requires architecture connection.

**Fix spec** — Rules plus a correction to an existing spec's behavior. Requires `reveals_gap_in`. Authoritative while active; merges into the originating spec on ship, then retires.

Every spec has two parts with different authority. The **commitment** (rules, behavior, constraints) is authoritative — code must match it. The **rationale** (hypothesis, stories, goals, why_now, findings) explains why the rules exist — predictive, not prescriptive.

**Shared rule** — A cross-cutting constraint across multiple specs. Extracted when the same rule appears in two specs.

**Work manifest** — The execution plan for a release. Derived from the release's specs and their lifecycle states. Breaks scope into claimable work items with dependencies and assignment. Lives alongside its release file. Initially generated by the `queue` script, then updated by agents and humans as work progresses.

**Work item** — A single claimable unit of execution within a work manifest. Each item maps to a skill trigger (draft-spec, generate-check, implement, resolve-drift, etc.). Tracks claiming, dependencies, and completion. Work items do not carry context or rationale — the spec does that. A work item is: what type of work, which spec, what state, who's on it, what's blocking it.

**Check** — A verification tied to a spec or shared rule. Method: `automated` (runs in CI), `manual` (human-executed script), or `property` (invariant).

### Evidence — did it work

**Finding** — A distilled research output. Validates planning assumptions, measures spec outcomes, or measures release outcomes. Categories: `user-research`, `analytics`, `technical`, `market`.

**Discovery** — A planned investigation to reduce uncertainty. Time-boxed. Lives in `direction/discoveries/` while in progress; emits a finding on completion and retires. No code is generated from a discovery.

### Process — how does the system run

The operating model itself: how the system runs, how agents operate within it, and how it improves over time.

**Agents file** — The routing table for agents. Declares what to load when, defers procedure to skills. Not a manual — a lookup structure. In Claude Code, this is `CLAUDE.md`.

**Skill** — A procedural recipe for a specific kind of work (drafting a spec, scoping a release, resolving drift). Lives in `process/skills/`. Loads only when its trigger fires.

**Script** — A mechanical verification tool. `validate` checks structural correctness, `drift` compares code against specs, `coverage` reports gaps, `graph` renders dependencies, `queue` generates and reconciles work manifests.

**Retro log** — A running record of friction and insight from agent work. Reviewed on release cadence. Humans act on the log; agents do not modify the agents file or skills based on it.

---

## Lifecycles

**Narrative, architecture, principles:** `draft` → `stable`

**Blueprints:** generated (no lifecycle; always reflects current shipped state). Curated blueprints, if used: `draft` → `stable`

**Goals:** `draft` → `active` → `achieved` → `retired`

**Stories:** `draft` → `validated` → `addressed` → `retired`

**Releases:** `drafting` → `active` → `shipped` → `retired`

**Specs (all types):** `draft` → `accepted` → `shipped` → `retired`

Fix specs retire on ship, after merging into the originating spec.

**Inline rules:** `proposed` → `binding` → `retired`

**Shared rules:** `proposed` → `binding` → `retired`

**Discoveries:** `planned` → `active` → `complete` → `retired`

**Findings:** `preliminary` → `supported` → `established` → `retired`

**Checks:** `draft` → `active` → `retired`

**Work items:** `blocked` → `ready` → `claimed` → `done` | `abandoned`

A work item starts `blocked` if it has unmet dependencies, otherwise `ready`. An agent claims it by writing its identifier and a timestamp. On completion, the item moves to `done`. If an agent drops work (timeout, rejection, failure), the item moves to `abandoned` and becomes claimable again. The `queue` script flags items that have been `claimed` beyond a configurable staleness threshold.

---

## Modes

Required rigor matches knowledge state. One flag in `.specs/config.yaml`:

**Pre-traction** (no real users, no baseline data): hypothesis required with confidence level, rules required, `why_now` required, stories inline or omitted, metrics accept `unknown` as current value, findings accept `preliminary` status, release hypothesis may target `establish-baseline`.

**Post-traction** (real users, real data): everything above, plus stories extracted and validated by findings, metrics must have real current values, curated blueprints recommended for complex user-facing flows, hypothesis check dates enforced, release hypotheses must specify concrete target metrics.

`validate` enforces the active mode.

---

# Part 3 — Operations

## Human and agent boundaries

Humans are accountable for framing, judgment, and acceptance. They write findings, accept specs, set goals, validate stories, flip release states, and approve work manifests. These are acts of judgment that carry authority.

Agents are primary operational users. They draft specs, extract rules, generate checks and code, detect drift, write retros, claim work items, and signal completion. They do not perform the acts listed above, and they do not modify the system about themselves.

Scripts handle mechanical correctness — validation, drift detection, coverage reporting, work manifest generation and reconciliation.

The boundary is about accountability, not capability. As agents become more capable, the set of tasks they perform grows. What stays with humans is the final say on intent, acceptance, and measurement.

---

## Agent operations

### Agents file

The agents file is the routing table for agents operating in the system. It tells agents what to load when, and defers procedure to skills. In Claude Code, this file is `CLAUDE.md`. See [reference.md](reference.md) for the full template.

The agents file contains: the loop, authority rules, boundaries, always-read context, context triggers, skill triggers, script triggers, parallel work guidance, and model defaults. It is a lookup structure — procedure lives in skills, not in the agents file.

### Skills

Skills live in `process/skills/` and carry procedure for specific kinds of work. Each skill declares its trigger and scope. A skill only loads when its trigger fires, keeping session context small.

Baseline skill set:

`draft-spec` — drafting a new spec of any type. `extract-shared-rule` — promoting a rule that appears in two specs. `generate-check` — adding checks to specs that lack them. `resolve-drift` — handling what `drift` flags. `scope-release` — drafting a new release file and its hypothesis. `plan-work` — generating and adjusting a release's work manifest. `write-release-finding` — aggregating spec outcomes into a release outcome. `plan-discovery` — choosing discovery over speculative spec work. `draft-fix-spec` — writing a fix spec and updating the originating spec. `lifecycle` — status transitions and their side effects. `legacy-evolution` — retiring, renaming, splitting, moving. `retro` — writing a stop/start/continue entry when friction or insight occurs.

Skills are added, modified, or split based on retro-driven review. New skills are introduced by humans.

### Entry protocol

**When given a specific task:**

1. Load always-read context
2. Read the spec; check `spec_type` and `release`
3. Load context triggered by the spec
4. Load skills triggered by the task
5. Do the work; parallelize where independent
6. Run the relevant script before committing
7. Mark the work item as `done` if one exists for this task
8. If friction or insight occurred, load the retro skill before finishing

**When looking for work:**

1. Load always-read context
2. Read the active release's work manifest
3. Find items with status `ready` (or `abandoned`)
4. Claim an item by writing agent identifier and timestamp
5. Follow the "given a specific task" protocol for the claimed item

Agents never browse the release map looking for work — they check the active release's work manifest. Agents never modify the agents file, skills, or the system itself based on their own retros.

---

## Code derivation and hypothesis accountability

### Code derivation

"Code is derived from specs" admits several implementations: full code generation from specs with `drift` enforcing match; partial scaffolding where specs generate module skeletons, tests, or API contracts; tests and checks generated from specs with production code written by humans and verified against generated checks; or loose derivation where specs are the authoritative reference, humans write code that matches, and `drift` compares asserted behavior.

All are valid. The common requirement: when code and spec disagree, the spec is updated first or the deviation is recorded. Never the code alone, silently.

### Hypothesis accountability

Shipped specs and releases have check dates. A finding measures the outcome as `confirmed`, `refuted`, or `inconclusive`. `coverage` reports shipped specs and releases past their check date with no measuring finding. All three outcomes are valid. A refuted hypothesis prevents doubling down on something that does not work.

Findings never change spec rules directly. A finding triggers a decision — draft a new spec, retire an existing one, adjust a goal, leave things as they are — made by humans.

---

## Scripts

**validate** — Frontmatter, link resolution, lifecycle states. Every accepted product spec references at least one story and one goal (post-traction). Every accepted architecture spec references architecture. Every accepted fix spec references a `reveals_gap_in` target. Every accepted spec references a release. Every active release has a hypothesis with success criteria. Curated blueprints flagged stale when shipped specs post-date them. Release map flagged stale if `current_release` doesn't match any active release.

**coverage** — Shipped specs past check date with no measuring finding. Shipped releases past check date with no measuring finding. Validated stories not addressed by any spec. Active goals with no findings measuring their metrics. Accepted specs with no checks. Clustered fix specs pointing at one originating spec. Unresolved retro entries older than 30 days.

**drift** — Compares generated code against accepted specs. Flags unrecorded drift. Recorded deviations are reported separately as debt.

**queue** — Generates initial work manifest from a release's spec list and spec lifecycle states. Reconciles manifest status against actual spec states (a spec that shipped makes its work items `done`). Flags stale claims (claimed longer than `stale_threshold` with no completion). Flags orphaned items (spec removed from release but item still in manifest). Flags dependency cycles. Reports abandoned items available for re-claim.

**graph** — Renders the dependency graph and generates the blueprint view from shipped specs. Filters: planning graph, delivery graph, hypothesis graph, evidence graph, blueprint.

---

## Review cadence

When a release ships:

1. A release finding is written measuring the release hypothesis
2. The retro log for that release window is reviewed and resolved
3. The next release is scoped based on both

The release finding answers "did the bet pay off." The retro log answers "did our operating model serve us." Both inform the next release.

---

# Part 4 — Adoption

## File structure

Structure grows with the work. Day one is small:

```
specs/
├── CLAUDE.md                    # agents file (name is tool-specific)
├── .specs/
│   ├── config.yaml              # mode: pre-traction
│   └── retro.log
├── direction/
│   ├── narrative.md             # short, always-read
│   ├── narrative-extended.md    # triggered when strategic framing is needed
│   ├── goals/
│   │   └── goal-*.md            # 2-3 goals
│   ├── discoveries/             # planned investigations in progress
│   └── findings/                # completed evidence
├── planning/
│   ├── release-map.md
│   └── releases/
│       ├── r1-[name].md
│       └── r1-[name].work.yaml  # work manifest, generated then maintained
├── domains/
│   └── [domain]/
│       └── spec-*.md            # 2-4 specs, all in r1
└── process/
    └── skills/
        ├── draft-spec.md
        ├── scope-release.md
        └── retro.md
```

The full structure, as it grows:

```
specs/
├── CLAUDE.md                    # agents file (name is tool-specific)
├── README.md                    # project readme (what the product is)
├── process/
│   ├── operating-model.md       # this document
│   └── reference.md             # page formats and examples
├── .specs/
│   ├── config.yaml              # mode: pre-traction | post-traction
│   └── retro.log
│
├── direction/
│   ├── narrative.md             # always (short version)
│   ├── narrative-extended.md    # strategic framing, on trigger
│   ├── architecture.md          # when technical shape is real
│   ├── blueprint.md             # generated by graph; curated version optional post-traction
│   ├── principles/              # when standards start repeating
│   ├── goals/                   # always (2-5 active)
│   ├── discoveries/             # planned investigations in progress
│   └── findings/                # completed evidence
│
├── planning/
│   ├── stories/                 # extracted when needs recur
│   ├── release-map.md           # always
│   └── releases/
│       ├── r[N]-[name].md       # one per release, from day one
│       └── r[N]-[name].work.yaml # work manifest, per active release
│
├── domains/
│   └── [domain]/
│       ├── spec-*.md
│       └── checks/
│
├── shared/
│   └── rules/                   # extracted when a rule appears in two specs
│
├── process/
│   ├── operating-model.md       # this document
│   ├── reference.md             # page formats and examples
│   ├── skills/                  # procedural recipes, loaded on trigger
│   └── scripts/                 # validate, drift, coverage, queue, graph
└── .specs/
```

Discoveries and findings live under `direction/` because they are framing material — evidence that informs how the team interprets specs and releases. The narrative is split into a short always-read `narrative.md` and a triggered `narrative-extended.md`; keeping the strategic framing out of the always-read payload keeps session context small.

**Extraction pattern.** Structure grows in response to real needs.

| When this happens | Extract this |
|---|---|
| Same story in two specs | Story file in `planning/stories/` |
| Same rule in two specs | Shared rule in `shared/rules/` |
| Generated blueprint insufficient for coordination | Curated blueprint in `direction/blueprint.md` |
| Three or more specs in one domain | Domain subfolder with its own checks |
| Standards repeating across specs | Principles file in `direction/principles/` |

---

## Legacy and evolution

**Retiring a page.** Set status to `retired`, add `replaced_by`. `validate` warns if non-retired pages reference it.

**Renaming.** Change filename and `id`, add `supersedes: old-id`.

**Curating a blueprint.** When the generated view isn't rich enough, create a curated `blueprint.md`. Keep specs' `journey` phase tags accurate so the generated view stays useful alongside it.

**Adding a domain.** Create the folder. No ceremony.

**Brownfield adoption.** Start with `narrative.md`, two or three goals, a first release file, one or two specs. Set mode honestly.

**Flipping modes.** When you have enough users to measure real metrics, flip `.specs/config.yaml` to `post-traction`.

**Unscheduled work.** A fix spec or small architecture spec may ship outside a release. Use `release: unscheduled`.

---

## Success criteria

- Every accepted product spec has a falsifiable hypothesis with direction, magnitude, and check timing
- Every active release has a hypothesis with success criteria
- Every shipped spec past its check date has a finding recording confirmed, refuted, or inconclusive
- Every shipped release has a release-level finding
- Every discovery produces a finding when complete
- Every validated story links to at least one finding
- Every active goal has metrics tracked by findings
- `validate` passes on every merged PR
- `drift` is clean, or drift is recorded as tracked debt
- The release map accurately reflects the current release state
- The retro log is reviewed every release cycle
- A new team member reads the five concepts, narrative, the current release, and one spec, and can review a PR
- Six months in, anyone can read r1 through r(N) in sequence and understand how the team's thinking evolved
- Multiple agents work on independent specs concurrently without coordination failures
- Agents can self-serve from the work manifest without human intervention per task
- Work manifests stay in sync with spec lifecycle states (`queue` reconciliation is clean)
- No stale claims persist beyond the configured threshold without resolution
- Humans and agents read the same artifacts without retranslation
- The operating model tightens over time through retro-driven iteration
