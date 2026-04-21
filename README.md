# Operating model

A spec-driven operating model for product development in the agentic age.

For decades, the tools of structured product development lived on one side of a wall. Source control, config-as-code, CI pipelines, schema validation: engineers worked in these systems daily. Everyone else worked in documents, slides, spreadsheets, and project boards. Two worlds, connected by meetings, handoffs, and the slow decay of context between them.

That wall is gone. When a PM runs an agent, the agent reads files, writes structured YAML, commits to git, and validates its own output. The PM doesn't need to learn git. The agent handles the tooling. The barrier that kept non-engineers out of the repo collapsed because agents made it irrelevant.

If everyone can work in the same repo, then everything that matters to product development can live there: north star, bets, hypotheses, specs, insights, evidence, retros, the planning itself. Source-controlled, diffable, reviewable, machine-readable. One connected system that evolves with the team.

Specs govern intended product behavior. They are the authoritative source for what the team has agreed to build. Other authorities exist (legal constraints, platform conventions, runtime reality, incident response) and the system interacts with them without subsuming them. A spec is a three-layer artifact: a qualitative **narrative** describing the slice of behavior the spec covers (explanatory, for humans and agents), machine-authoritative **rules** that code must match at status `binding`, and enforced **checks** — at least one per binding rule, so that the set of all enforced checks across all binding rules is the regression suite by construction. Narrative is explanatory, not authoritative; when narrative and rules conflict, rules win. The theory of change — why these rules will produce the outcome we want — lives one level up, on the initiative that produced the specs, and one level above that, on the bet the initiative tests. A spec is neutral on existing-vs-new: it may codify behavior that already exists in the product, describe new behavior the product will honor after an initiative ships, or both.

> This doc covers the model. For schemas, lifecycles, file layout, git mechanics, the script inventory, and agent entry protocols, see [reference.md](reference.md).

---

## The artifacts

Six artifacts carry most of the weight. Everything else is optional structure that emerges when real work demands it.

**Direction.** The framing material shared by all collaborators — narrative, north star, architecture, principles, insights. Lives under `direction/`. Layered by rate of change: narrative and north star change rarely; architecture and blueprints change when things ship; principles change as learning accumulates.

**Bet.** A strategic claim with a hypothesis, kill criteria, metrics, and signals. The durable layer — bets persist across multiple tactical pilots, accumulating evidence over time. A bet moves to `completed` when its hypothesis has been answered (confirmed, refuted, or deferred), not when a particular piece of work ships.

**Initiative.** A tactical pilot under one or more bets (M:N). Carries a falsifiable hypothesis, success criteria, and kill criteria. Four types: `product` addresses a user need; `architecture` improves a system property; `fix` corrects a gap in a shipped spec; `discovery` is a time-boxed investigation that leaves `specs_addressed` empty and emits an insight instead of code. Tasks parent on an initiative — it is the execution unit.

**Spec.** The smallest authoritative coordination unit for a code contract — a narrow, durable slice of behavior. Three layers: a qualitative **narrative** (explanatory, for humans and agents), machine-authoritative **rules** (code must match rules at status `binding`; rule lifecycle `proposed → binding → retired` handles evolution in place), and **checks** (at least one enforced check per binding rule — the regression suite by construction). No hypothesis, no kill criteria, no story links on the spec itself — those live on the initiative that addresses it. A spec is neutral on existing-vs-new: it may codify behavior that already exists, describe new behavior, or both. Code derives from accepted specs; unrecorded drift is an error. When code and spec disagree, the spec is updated first or the deviation is recorded — never the code alone, silently. 

**Task.** The execution primitive. A claimable, agent-executable unit of work parented on an initiative, with an optional `updates_spec` side effect. Every state change (claim, complete, verify, abandon) is a structured commit on the `plans` branch, mediated by a script.

**Insight.** A distilled piece of evidence. Measures a bet or an initiative against its hypothesis — `confirmed`, `refuted`, or `inconclusive`. All three outcomes are valid; a refuted hypothesis prevents doubling down on something that doesn't work. Insights accumulate forever as the durable evidence ledger. They don't measure specs directly — specs have no hypothesis, so drift is the spec's "did the rules hold?" signal.

The flow connects them: bets articulate strategic claims; discovery initiatives reduce uncertainty when evidence is thin; product, architecture, or fix initiatives commit specs and ship them spec-by-spec through PRs; insights close the loop back to the bets. Two measurement layers — *did this pilot pay off?* (initiative insight) vs *is the strategic bet holding up?* (bet insight) — inform what gets drafted next. Coordinated shipping of several specs together is expressed by grouping them under one initiative, not by a release.

The three "where we're going" axes that used to exist (goal, initiative, release) collapse into two: **bet** for the durable strategic claim, **initiative** for the tactical pilot.

---

## Three layers of collaboration

The system serves three kinds of collaborator, and all three do load-bearing work.

**Humans.** Frame intent, judge, prioritize, accept. Humans promote insights from `preliminary` to `supported` / `established`, accept specs, flip bet and initiative status, validate stories, and approve task schedules. The artifact — not a meeting — carries the agreement between humans; alignment happens by reading.

**Agents.** Primary operational users of the system. Read the spec, draft bets, draft initiatives, draft specs, extract rules, generate checks and code, detect drift, write insights, claim tasks, complete them, verify each other's work. Multiple agents work in parallel on different specs without coordinating directly, because the specs themselves carry bounded context and the task queue enforces explicit dependencies.

**Scripts.** The toolbelt that does deterministic work — validation, state transitions, drift detection, sync — as code rather than as prompts. Orders of magnitude cheaper than agent labor, and scripts don't hallucinate. Every task state change runs as a script because the cost of a bad state commit is high and the cost of a script is nil.

The division of labor: **humans curate, agents draft, scripts orchestrate and verify.** Every procedure the system relies on should be pushed as far down that stack as it can go without losing judgment.

The full script inventory, git coordination, and agent entry protocols live in [reference.md](reference.md).

---

## Principles

**Specs are the authoritative source for intended code behavior — a three-layer artifact.** A spec has a qualitative narrative (explanatory), machine-authoritative rules (code must match rules at `binding`), and enforced checks (at least one per binding rule). Rules are the contract; narrative helps humans orient; checks are the regression suite by construction. Unrecorded drift is an error. Recorded deviation is tracked debt. When narrative and rules conflict, rules win — update the narrative to match.

**Hypothesis lives on bets and initiatives, not specs.** Rules are authoritative and prescriptive. Hypothesis is predictive and belongs one layer above the rules — it explains why these rules will produce the outcome, and it's the thing insights close against.

**Every initiative is a bet. Every bet is a bigger bet.** Tactical initiatives rarely move metrics alone; their insights accumulate evidence for the strategic bet above them.

**Four reasons to draft an initiative.** Product, architecture, fix, discovery. A discovery initiative reduces uncertainty before committing — when evidence is thin, draft a discovery first; it produces an insight, not code.

**Prefer scripts over agent labor for mechanical work.** If a check, transformation, or state transition is deterministic, it belongs in a script. Agents have better things to do than re-derive what a twenty-line script can compute, and scripts don't hallucinate.

**Structure is extracted, not written upfront.** Duplication is the forcing function. When the same story, rule, or journey appears in multiple places, extract it.

**Change the source of truth first.** Never patch generated code silently. Update the spec, regenerate, verify. When deviation is unavoidable, record it. The spec stays authoritative.

**Bounded context is an interface.** Collaborators read what the spec links to, nothing more. The spec is the routing layer. The agents file is a routing table, not a manual.

**Parallelism is a design goal.** Multiple agents work safely in parallel because the system provides stable references, explicit authority, task queues with declared dependencies, and a two-agent verification step at the boundaries.

**The system learns about itself.** Friction and insight are logged in retros. We review and act on the log. The operating model tightens over time.

---

## First touch of an un-specced behavior

When an initiative wants to change a behavior that already exists in code but has no spec covering it, the first task in the queue should be a `draft-spec` task that codifies today's behavior. The trigger is **spec absence for the specific behavior being changed**, not novelty of the broader product domain — a coarse domain (coach, member) can contain many existing specs and still have un-specced behaviors that a new initiative wants to touch. Codifying first gives regression protection (existing checks stay green as the initiative evolves the spec), makes the change visible as a git diff against a stable baseline, and limits the blast radius to rules actually touched. Codification uses the ordinary `draft-spec` skill — no new machinery.

---

## Spec granularity and decomposition

A spec covers one coherent slice of behavior — roughly the size of a feature (think `spec-task-reschedule`, `spec-coach-login`, `spec-video-check-in`). Coarse product domains (coach, member) are too broad to be the spec unit. When a slice grows to the point that its rules start clustering into distinct sub-concerns, **decompose into multiple narrow peer specs under the same domain folder — no sub-specs**. Reasons: rule-level lifecycle (`proposed → binding → retired`) already handles intra-spec evolution; shared rules (`shared/rules/`) handle cross-spec duplication; the domain folder (`specs/[domain]/`) handles navigational grouping once three or more specs accumulate. A spec's narrative should make the slice's *edges* crisp — what this spec covers vs. what a peer spec covers.

---

## Contrast with feature-scoped SDD

Spec-driven development in the mould of [spec-kit](https://github.com/github/spec-kit) scopes a spec to one feature, numbered sequentially (`001-add-reset`, `002-add-sso`), with tests derived per feature and brownfield / drift handled by community extensions. This model differs on six points:

1. **Narrow-scoped *durable* specs with rule-level lifecycle.** Each spec covers a coherent slice of behavior (roughly the size of a spec-kit feature spec), but it *persists* across many iterations rather than being replaced or superseded per feature. A single spec absorbs many changes via the rule lifecycle (`proposed → binding → retired`).
2. **Checks as a persistent regression substrate.** One enforced check per binding rule. Checks persist with their rules; they don't retire with the feature that introduced them.
3. **Hypothesis + insight measurement loop.** Bets (strategic) and initiatives (tactical) carry falsifiable hypotheses; insights close the loop with `confirmed | refuted | inconclusive`. Feature-scoped SDD has no equivalent.
4. **Existing-vs-new neutrality in the core.** A spec can codify existing behavior; first-touch-of-an-un-specced-behavior is a named pattern in the core doctrine, not a brownfield extension.
5. **Drift as a first-class script with a codified drift contract.** `/drift` runs continuously; `update-spec` forbids silent in-place rewriting of `binding` rules (retire-with-replaced-by instead).
6. **Ambiguity surfaced inline as `## Open questions` on the draft.** Rather than a separate `/clarify` command whose output disappears once passed, open questions live in the spec body — versioned, reviewable, and cleared (or explicitly deferred) before promotion to `accepted`.

**Feature-sized change translation.** The mental map for a feature-scoped SDD reader: "one feature = one spec" becomes **"one feature = one initiative → touches one-or-more narrow specs"**. The initiative carries the feature's hypothesis and scope (and is the thing that completes); the specs it touches carry the durable contract (and outlive the initiative). A change like "add SSO" is one initiative that touches one or more narrow specs (e.g., `spec-coach-login`, possibly a new peer `spec-coach-scim-provisioning`).

---

## Non-goals

The system is designed for product development where specs govern intended behavior and evidence closes the loop. It is not:

- Required for throwaway prototypes with no intent to learn
- A replacement for legal, security, or compliance review
- A replacement for incident response or operational runbooks
- Suited to one-line fixes that don't reveal a gap in a spec's rules
- A batched release train — the model is deliberately release-less (super-agile); specs ship spec-by-spec
- An attempt to govern runtime behavior, platform constraints, or any authority outside intended code behavior

An initiative is warranted when a tactical bet is worth recording with a falsifiable hypothesis. A spec is warranted when a change is meaningful enough that its rules will be referenced by future work. Below that threshold, use the issue tracker.
