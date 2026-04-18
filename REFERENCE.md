# Reference

Page formats, YAML schemas, and templates for the spec-driven operating model. See [README.md](README.md) for the model itself.

---

## Config

```yaml
# .specs/config.yaml
mode: pre-traction   # or: post-traction
```

---

## Release map

```yaml
id: release-map
type: release-map
status: active
current_release: r3-collaboration
next_release: r4-insights
updated: 2026-04-18
```

```markdown
## Release state

| Release | Status    | Window            | Primary goals                          |
|---------|-----------|-------------------|----------------------------------------|
| r1      | shipped   | 2026-02 – 2026-03 | goal-task-completion, goal-capture-speed |
| r2      | shipped   | 2026-03 – 2026-05 | goal-daily-return                      |
| r3      | active    | 2026-05 – 2026-07 | goal-shared-lists                      |
| r4      | drafting  | 2026-07 –         | goal-review-habit                      |
```

---

## Release

```yaml
id: map-r1-core-loop
type: map-release
status: active
title: Core loop
window_start: 2026-02-01
window_end: 2026-03-15
primary_goals: [goal-task-completion, goal-capture-speed]

hypothesis:
  statement: "A functional quick-add + task list + due-date flow will demonstrate that users can capture and complete tasks faster than their current tool and establish a completion-rate baseline"
  confidence: medium
  confidence_basis: "User interviews confirm friction with existing tools; no quantitative baseline yet"
  metrics: [avg-capture-time, task-completion-rate]
  direction: establish-baseline
  success_criteria:
    - "Baseline exists for both metrics"
    - "Median capture time < 3s for returning users with defaults"
  check_after: 4w-after-ship

specs_in_scope: [spec-quick-add, spec-task-list, spec-due-dates]
stories_addressed: [story-capture-task, story-see-whats-next, story-set-deadline]
```

Body sections: **Scope**, **Rationale**, **Decisions and deferrals**, **Dependencies**, **Risks**.

When shipped: `window_end` locks, a release finding is written, the retro log for the window is reviewed.

---

## Goal

```yaml
id: goal-task-completion
type: goal
status: active
title: Users complete tasks they capture rather than abandoning them
connected_stories: [story-capture-task, story-see-whats-next]
kill_criteria: "If completion rate stays below 40% after three cycles, retire and reassess"
```

Body: Goal / Signals / Metrics table with current values and sources.

---

## Story

```yaml
id: story-reschedule-task
type: story
status: validated
journey_phase: review
supports_goals: [goal-task-completion]
refs_findings: [finding-overdue-task-causes]
```

```markdown
**When** I open my list and see overdue tasks I couldn't get to,
**I want to** reschedule them to a realistic date,
**so I can** trust my list instead of ignoring it.
```

---

## Product spec

```yaml
id: spec-quick-add
type: spec
spec_type: product
status: accepted
title: Quick-add task capture
domain: capture
release: r1-core-loop

hypothesis:
  statement: "A single-field quick-add with smart defaults will get capture time below 3 seconds"
  confidence: medium
  confidence_basis: "12 user interviews confirm slow capture is the top reason people stop using task apps"
  metric: avg-capture-time
  direction: decrease
  from: unknown
  to: "<3s"
  check_after: 4w

story_inline: "When something comes to mind, I want to capture it in seconds without choosing a project or date, so I don't lose the thought."
supports_goals: [goal-capture-speed, goal-task-completion]

journey: journeys/capture
refs_shared_rules: []
refs_principles: [design]
refs_findings: [finding-capture-friction-interviews]

why_now: "The entry point. If capture is slow, nothing else matters."
```

```markdown
## Rules
<!-- Authoritative. Code must match. -->

### Single-field entry
Tasks are created from a single text field. No required fields beyond title.
Status: binding

### Smart defaults
New tasks default to inbox (no project), no due date, normal priority.
If the user types a date-like phrase ("tomorrow", "friday"), it is parsed into a due date.
Status: binding

## Hypothesis
<!-- Predictive. Does not affect rule authority. -->

Single-field entry with smart date parsing will reduce median capture 
time below 3 seconds within 4 weeks of ship.

## Deviations
_None yet._
```

---

## Architecture spec

```yaml
id: spec-offline-sync
type: spec
spec_type: architecture
status: accepted
title: Offline-first sync for task operations
domain: sync
release: r2-daily-return

hypothesis:
  statement: "A local-first data layer will let all task operations succeed without connectivity and sync on reconnect without data loss"
  confidence: high
  property: reliability
  success_criteria:
    - All task operations succeed with zero connectivity
    - Sync completes within 10s of reconnect
    - No data loss on conflict

supports_goals: [goal-daily-return]
refs_architecture: true
why_now: "Users who lose a task once stop trusting the app. Retention risk confirmed by r1 exit analysis."
```

---

## Fix spec

```yaml
id: spec-fix-reschedule-clears-recurrence
type: spec
spec_type: fix
status: accepted
title: Rescheduling should not clear recurrence rule
domain: scheduling
severity: medium
release: r3-collaboration
reveals_gap_in: spec-recurring-tasks

hypothesis:
  statement: "Preserving recurrence when rescheduling eliminates the confusing behavior where recurring tasks stop repeating"
  success_criteria: "Rescheduling a recurring task moves the instance without affecting the recurrence rule"
```

Rules authoritative while `accepted`. On ship, the originating spec (`spec-recurring-tasks`) absorbs the rules; the fix spec retires.

---

## Discovery

```yaml
id: discovery-overdue-task-behavior
type: discovery
status: active
title: How users handle overdue tasks
method: "Diary study (30 participants, 2 weeks) + 6 follow-up interviews"
deadline: 2026-05-30
produces_finding: finding-overdue-task-causes
informs_anticipated: [story-reschedule-task, story-bulk-triage]
```

On completion, emits a finding and retires.

---

## Finding

```yaml
id: finding-r1-release-outcome
type: finding
status: established
title: R1 core loop release outcome
category: analytics
measures_release: r1-core-loop
measures_hypothesis: [spec-quick-add, spec-task-list, spec-due-dates]
result: inconclusive
```

Body: summary, implication, links to raw data.

Hypothesis accountability fields:

```yaml
measures_hypothesis: spec-quick-add
result: confirmed   # confirmed | refuted | inconclusive
```

```yaml
measures_release: r1-core-loop
result: inconclusive
```

---

## Shared rule

```yaml
id: rule-api-response-format
type: rule
status: binding
title: API response format standard
applies_to: [spec-quick-add, spec-task-list, spec-recurring-tasks]
```

---

## Check

```yaml
id: check-quick-add-defaults
type: check
status: active
method: automated
verifies: [spec-quick-add]
runner: tests/capture/defaults.test.ts
```

---

## Work manifest

```yaml
id: work-r3-collaboration
type: work-manifest
release: r3-collaboration
generated: 2026-05-10
updated: 2026-05-12
stale_threshold: 4h
```

```yaml
queue:
  - id: w-fix-reschedule-recurrence
    type: draft-fix-spec
    spec: spec-fix-reschedule-clears-recurrence
    status: done
    completed_at: 2026-05-11T09:00:00Z

  - id: w-implement-shared-lists
    type: implement
    spec: spec-shared-lists
    status: claimed
    agent: agent-2
    claimed_at: 2026-05-12T14:30:00Z
    blocked_by: []

  - id: w-generate-checks-shared-lists
    type: generate-check
    spec: spec-shared-lists
    status: blocked
    blocked_by: [w-implement-shared-lists]

  - id: w-drift-shared-lists
    type: resolve-drift
    spec: spec-shared-lists
    status: blocked
    blocked_by: [w-implement-shared-lists]
```

Work item `type` values map to skill triggers: `draft-spec`, `extract-shared-rule`, `generate-check`, `implement`, `resolve-drift`, `draft-fix-spec`, `write-release-finding`. If a work item doesn't map to an existing skill, it belongs in an issue tracker, not the manifest.

`blocked_by` references other work item IDs within the same manifest. Cross-release dependencies should be rare; when they exist, the blocking item is referenced by its full `release/item` path.

The `queue` script generates the initial manifest from the release's spec list and their lifecycle states. Humans review and adjust dependencies and ordering. After that, agents claim and complete items as they work.

---

## Agents file template

The full agents file for use in a repo. In Claude Code, save as `CLAUDE.md`.

```markdown
# Agents file

Spec-driven collaboration system. Specs govern intended product behavior. 
Code is derived from specs. Unrecorded drift is an error; recorded deviation 
is debt.

This file is a routing table. Load skills for procedure; load context files 
only when triggered. If detail feels missing here, it belongs in a skill.

## The loop
Frame → discover → define → scope → commit → plan → implement → ship → measure → reflect.

## Authority
Rules = authoritative (code must match).
Hypothesis and rationale = predictive (explain why, don't bind code).

## Boundaries
Agents draft specs, extract rules, generate checks and code, detect drift, 
write retros, claim and complete work items. Agents do not write findings, 
accept specs, set goals, validate stories, approve work manifests, or flip 
release states.

## Always read
- context/narrative.md
- .specs/config.yaml

## Context triggers
- Touches system shape → context/architecture.md
- References a journey → context/blueprint.md (or context/blueprints/[phase].md)
- References principles → context/principles/[name].md
- Release-level or cross-spec → planning/release-map.md
- Working on a spec → the spec + the release it references
- Looking for work or claiming → the active release's work manifest

## Skill triggers
- Drafting a spec → draft-spec
- Rule appears in two specs → extract-shared-rule
- Spec lacks checks → generate-check
- drift flagged something → resolve-drift
- New release needed → scope-release
- Release committed, needs work breakdown → plan-work
- Release shipped → write-release-finding
- Evidence is thin → plan-discovery
- Bug surfaced → draft-fix-spec
- Changing lifecycle status → lifecycle
- Retiring or renaming → legacy-evolution
- Friction or insight during a task → retro

## Script triggers
- Before committing spec changes → validate
- Before committing code changes → drift
- Weekly or monthly review → coverage
- Tracing or visualizing → graph
- Release committed or work state unclear → queue

## Parallel work
Claim work items from the active release's work manifest. Work on 
independent items in parallel. The system provides bounded context, 
stable references, and verification at the edges. Serialize only on real 
dependencies declared in blocked_by. If no manifest exists, work on 
independent specs directly — the manifest is an accelerant, not a gate.

## Model defaults
Stronger models for drafting and judgment. Cheaper models for mechanical 
verification and transformations.
```
