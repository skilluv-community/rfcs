# RFC 0000 — Governance (meta-RFC)

- **Date**: 2026-07-21
- **Author(s)**: @jeremie0342
- **Status**: Accepted
- **Discussion window**: N/A (bootstrap RFC)
- **Related**: [Skilluv Community Charter](https://github.com/skilluv-community/community-charter)

## Summary

This meta-RFC defines how Skilluv makes decisions. It is the process by which all other RFCs (and all governance evolutions) are decided. It establishes:

- Three levels of decisions (Strategic / Operational / Community-facing)
- Who holds decision authority at each level during 2027 (bootstrap phase)
- The evolution roadmap toward committee-based governance in 2028+
- Transparency mechanisms (RFCs, community polls, weekly changelog, quarterly AMA)

## Motivation

Skilluv is a compagnonnage tech platform in bootstrap phase. Governance must be:
- **Clear** — everyone knows who decides what and why
- **Transparent** — decisions are visible, justified, historized
- **Progressive** — starts founder-driven, evolves toward community-committee
- **Documented** — future contributors understand the reasoning behind past decisions

Without a documented governance process, decisions are opaque, contested, and forgotten. This RFC formalizes the process.

## Detailed proposal

### Three levels of decisions

| Level | Type of decision | Who decides in 2027 | Who decides from 2028+ |
|---|---|---|---|
| **A — Strategic** | Product vision, economic model, structural partnerships, key hires, legal changes | **Founder alone** | Founder + advisory council |
| **B — Operational** | Tier 3 sanctions, major publications, charter adjustments, tactical roadmap | Founder + 1-2 trusted admins | **Steward committee** (5-7 elected) |
| **C — Community-facing** | Future season themes, deliverables proposals, OSS partner additions, feature prioritization, UX/UI | **Non-binding community poll + founder decision** | Non-binding polls + committee decision |

### Polls

- **Voting basis**: 1 person = 1 vote (no weighting by rank)
- **Voting window**: 7 to 30 days depending on stakes
- **Publication**: real-time results + final decision with rationale
- **Founder may go against poll majority** but must publish rationale

### Poll types

- Prioritization — which seasonal deliverable to launch first?
- Multiple choice — which name for a future season?
- Yes/No consultative — should we add technology X to the official stack?
- Ranking — rank these flagship candidates by priority

### Transparency mechanisms

1. **This RFC repository** — every structural decision is an RFC.
2. **Community polls** — for level C decisions.
3. **Weekly changelog** — published each Friday at [skilluv-community/changelog](https://github.com/skilluv-community/changelog).
4. **Quarterly AMA** — founder answers community questions publicly on the forum for 48 hours.
5. **Annual retrospective** — public document at Skilluv Fest (December): "what worked this year, what didn't, what we're adjusting."

### Line of succession

**Short-term absence** (vacation, indisposition):
- Founder officially designates 1-2 "vice-admins" for the period
- They can decide Level B (operational) only
- Level A frozen until founder returns
- Publication of the vice-admin on duty on the forum

**Long-term absence** (extended incapacity — hypothetical for 2027):
- Community succession RFC opened
- Provisional committee elected
- To be formalized before 2028 (not urgent day-1)

### Conflict of interest

If the founder or any decision-maker has a conflict of interest (personal financial stake, competitor role, etc.):
- **Recusal mandatory** from the decision
- Decisions delegated to trusted admins or committee
- **Conflict published** in the affected RFC (radical transparency)

### Evolution roadmap

| Timeline | Governance state |
|---|---|
| **2027 S1** | Founder alone (Levels A + B), community polls non-binding (Level C), RFC repo created, AMA quarterly, weekly changelog |
| **2027 S2** | Recruitment of an informal advisory council (3-5 people, no decision power), first co-stewards for flagships |
| **2028 S1** | **Steward committee** (5-7 elected people) takes Level B, founder keeps Level A, advisory council becomes official |
| **2028 S2** | Level C polls become binding (community actually decides on some questions) |
| **2029+** | Community-driven Level C, committee Level B, founder keeps veto on Level A while founder |

## Alternatives considered

**Alternative A — Fully community-driven from day 1** (Wikipedia model).
Not retained because Skilluv has no critical mass to sustain community-decision-making yet. A single person needs to hold the vision cohesive during bootstrap.

**Alternative B — Benevolent dictator model** (Linux, Python-style).
Partially adopted (founder-led in 2027), but with an explicit evolution roadmap toward committee. Skilluv does not want to be dependent on one person forever.

**Alternative C — Corporate hierarchical model**.
Not retained. Skilluv is a compagnonnage community, not a company. Governance must feel like a workshop, not an office.

**Alternative D — Doing nothing (implicit governance)**.
Not retained. Explicit governance is a prerequisite for community trust and long-term sustainability.

## Impact

- **Users** — understand who decides what, can contribute via RFCs and polls
- **Contributors** — clear process to propose changes
- **Mentors** — know the decision structure, can eventually join advisory council or steward committee
- **Enterprises** — trust the platform, know the founder has decision authority in 2027
- **Long-term sustainability** — Skilluv survives founder unavailability via succession + committee evolution

## Rationale

This RFC is accepted as the bootstrap governance document. It will be amended by future RFCs as the community grows.

---

## Post-decision

**Decision date**: 2026-07-21
**Decision by**: Founder (bootstrap phase)
**Result**: Accepted

**Implementation tracking**: this repository itself. Future governance RFCs will amend this document via versioning.
