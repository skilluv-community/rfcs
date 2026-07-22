# RFC 0001 — OSS Partnership Tiers: Promotion by Proof

- **Date**: 2026-07-22
- **Author(s)**: @jeremie-zitti
- **Status**: Draft
- **Discussion window**: 30 days (structural)
- **Related**: RFC 0000 (Meta-governance)

## Summary

This RFC formalizes a three-tier classification for the OSS projects Skilluv curates and mentors contributions to. It establishes a clear rule: **projects are only promoted between tiers based on accumulated, opposable proof — never based on aspiration or stated intent**.

Concretely:

- **Tier 1** — unilateral curation (default entry). Skilluv lists the project, guides contributors toward it. No relationship with maintainers required.
- **Tier 2** — light partnership: an outreach email has been sent, at least one Skilluv-labelled contribution has been merged, a maintainer has acknowledged.
- **Tier 3** — formal partnership (MoU): a signed agreement exists, with defined benefits to both sides (mentorship, labels, hiring pipeline, co-branded events, etc.).

Promotion is one-way, evidence-based, and reversible.

## Motivation

The temptation for any young platform is to inflate its perceived importance by classifying partners aspirationally. A landing page reading "In partnership with sqlx, Cal.com, Meilisearch" carries obvious marketing value — and is almost always dishonest at launch.

The problem is not just ethical. It compounds:

1. **Reputation risk**: a maintainer who discovers they are listed as a "partner" without agreement will publicly clarify. That signal spreads fast in OSS communities.
2. **Contributor confusion**: Skilluv talents who show up on a "partner" project expecting a warm welcome, and instead receive the standard cold-open contributor experience, will feel misled by us — not by the project.
3. **Long-term dilution**: if "partnership" means nothing measurable, we lose the ability to distinguish real partnerships when they eventually happen. The word ceases to signal.

The rule adopted here — **promotion by proof only** — is a deliberate constraint. It makes Tier 3 rare, aspirational, and worth pursuing. It makes Tier 2 evidence of actual work. It makes Tier 1 honest about what it is: our editorial choice, unilaterally.

## Detailed proposal

### Tier 1 — Unilateral curation

**Meaning**: Skilluv, as editor, believes this project is a good pedagogical fit for our contributors. We curate its issues, we prepare mentors, we direct contributors toward it. We claim nothing about the project's endorsement of Skilluv.

**Database representation**:
- `projects.curated_by_admin = true`
- `projects.skilluv_partnership_level IS NULL`
- `projects.skilluv_editorial_notes` may describe the pedagogical intent, but must not imply a two-way relationship

**Public display**: "Featured project" or "Curated for compagnonnage" — never "partner".

**Required proofs to enter Tier 1**: editorial decision by a Skilluv Steward. No external evidence required.

### Tier 2 — Light partnership

**Meaning**: A named maintainer of the project has been contacted by Skilluv, has acknowledged Skilluv's existence, and has expressed at minimum a neutral or positive receptivity. At least one Skilluv contributor has landed a merged PR that the maintainer has publicly recognized or that a Skilluv mentor has co-signed.

**Database representation**:
- `projects.skilluv_partnership_level = 2`

**Public display**: "Skilluv Community Partner (Tier 2)" — with an explicit link to the archived correspondence (see below).

**Required proofs to enter Tier 2** (all must hold):

1. **Documented outreach**: a public email or issue thread archived in `skilluv-community/changelog` referencing the initial contact and the maintainer's response. Screenshots not accepted — links to original threads only.
2. **Merged contribution**: at least one merged PR from a Skilluv contributor with the `skilluv-community` label or a mention of Skilluv in the description.
3. **Maintainer acknowledgement**: an explicit written statement from a maintainer that they are aware of Skilluv and do not object to the Tier 2 designation. This can be minimal — a "yes, that's fine" in a public thread is sufficient. It must be linkable.

Any of these three going stale (maintainer withdraws, thread deleted, contribution unmerged) automatically demotes the project to Tier 1.

### Tier 3 — Formal partnership

**Meaning**: A written agreement exists between Skilluv and the project's governance (individual maintainer, foundation, or company). The agreement specifies mutual benefits, term, and conditions of termination.

**Database representation**:
- `projects.skilluv_partnership_level = 3`

**Public display**: "Skilluv MoU Partner" — with a link to the public summary of the MoU (not the full document).

**Required proofs to enter Tier 3** (all must hold):

1. **Signed MoU** (or equivalent binding written agreement), archived internally, summarized publicly.
2. **Named liaison** on both sides.
3. **Duration and renewal** clauses. No open-ended MoUs.
4. **Public opt-out clause**: either party can terminate with 60 days' notice.
5. **Existing Tier 2 status for at least 90 days** before Tier 3 upgrade. No skipping tiers.

Termination of the MoU automatically demotes to Tier 2 for a 90-day grace period, then to Tier 1 unless renewed.

### Demotion rules

Any tier can be demoted:

- **Automatic demotion** occurs when the required proofs cease to hold (see per-tier lists above).
- **Manual demotion** can be requested by any Skilluv contributor via a governance issue on this repository. The demotion is discussed openly and decided by the Steward committee within 14 days.
- **Voluntary demotion** occurs when a maintainer asks Skilluv to no longer list their project or downgrade its tier. This is always honored within 7 days.

Demotion is not punitive. It is a factual update of what is currently true.

### Tier badge display in UI

The Skilluv frontend displays tier badges with the following conventions:

- Tier 1: neutral grey "Curated"
- Tier 2: soft green "Community Partner"
- Tier 3: solid gold "MoU Partner"

Tiers must never appear next to a project without a link to the public evidence page (Tier 2/3) or the editorial note (Tier 1).

## Alternatives considered

**Alternative A — No tier system, all projects equal.**

Rejected: Skilluv's differentiation is editorial curation. A flat listing dissolves the signal we're trying to build. Contributors also benefit from knowing which projects have active Skilluv-partner support versus which are self-served.

**Alternative B — Two tiers only (Curated / Partner).**

Rejected: it collapses the meaningful distinction between "we've talked and they know" (Tier 2) and "we've signed" (Tier 3). Since the effort to reach each tier differs dramatically, they deserve distinct labels.

**Alternative C — Marketing-driven tiers with looser proof requirements.**

Rejected. See Motivation. The proof requirements are the point.

**Alternative D — Doing nothing (current state: informal editorial designation).**

Rejected as unstable. Without a rule, the platform will drift toward inflating perceived importance as it grows. Codifying now is cheaper than backpedaling later.

## Impact

**Impact on users (contributors)**: Positive. Users can trust tier labels. They know Tier 2/3 means real support is available; Tier 1 means "great project, mentored by Skilluv but no direct maintainer channel".

**Impact on mentors**: Positive. Mentors have a clear rubric for what promises they can make on Skilluv's behalf.

**Impact on enterprises (hiring side)**: Positive. Enterprise recruiters can filter for Tier 2/3 experience as a stronger signal.

**Impact on maintainers of curated projects**: Neutral to positive. Tier 1 makes no claims. Tier 2/3 requires their explicit consent. No maintainer is ever surprised by their designation.

**Impact on the codebase**:
- `projects.skilluv_partnership_level` (added in migration 0112) is the sole source of truth.
- Admin CRUD (`/projects`) must enforce that Tier 2 and Tier 3 promotions require a linked evidence URL (proposed follow-up in a separate technical RFC).

**Impact on ongoing work**: All 12 currently curated partners start at Tier 1. No project starts higher, including projects where the founder has a pre-existing personal relationship with a maintainer.

## Open questions

1. **Who is the Steward committee?** At time of writing, there are no active Skilluv stewards other than the founder. This RFC assumes a future Steward role that does not yet exist. Should the tier promotion process be founder-only until the steward role is populated? Community input requested.

2. **Public evidence archive format.** Where and how do we archive the Tier 2 "maintainer acknowledgement" links? Proposed: a dedicated `evidence/` directory in this RFC repository, or in `skilluv-community/changelog`. Preference?

3. **Tier retention audits.** How often should we re-verify Tier 2/3 proofs still hold? Proposed: annually, with an automated tool that pings each linked URL. Alternatives welcome.

4. **Handling of dormant projects.** If a Tier 2 project's maintainer becomes unresponsive for >12 months, does the project stay Tier 2, drop to Tier 1, or acquire a special "dormant" label? No strong opinion yet.

## Rationale

_This section will be filled in after the discussion window closes._

---

## Post-decision

**Decision date**: [pending]
**Decision by**: [pending — founder-only interim, or committee once populated]
**Result**: [pending]

**Implementation tracking**: [pending — will link to skilluv-backend and skilluv-admin follow-up issues]
