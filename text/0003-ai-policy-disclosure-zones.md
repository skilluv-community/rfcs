# RFC 0003 — AI Policy: Disclosure Zones and Human Verification

- **Date**: 2026-07-22
- **Author(s)**: @jeremie-zitti
- **Status**: Draft
- **Discussion window**: 30 days (structural, addresses core positioning)
- **Related**: RFC 0000 (Meta-governance), community-code-of-conduct

## Summary

This RFC codifies Skilluv's public position on the use of AI coding assistants (Claude, Copilot, ChatGPT, Cursor, and successors) by contributors. The position is deliberate and inversion-resistant:

- **Disclosure is required. Prohibition is not.**
- **Three zones with distinct rules**: *learning*, *real project*, *tournament*.
- **One and only one control point**: `human_verified` capstones at end of season.
- **No public classification of contributors based on AI usage.** No AI-usage badges, no AI-transparent tier ranking, no leaderboards splitting "human-only" from "AI-assisted" contributors.

The claim: Skilluv's differentiator is not "no AI here" (impossible to enforce, dishonest at scale). It is "we know when and why AI shaped the artefact, and we teach how to work with it".

## Motivation

Every code-contribution platform faces the same choice on AI:

1. **Ban it, pretend it doesn't exist.** Cheating happens invisibly, moderation becomes surveillance, credibility collapses the first time a top contributor is publicly caught.
2. **Endorse it uncritically.** The platform becomes AI-generation-with-a-wrapper. Contributor skill claims lose all meaning.
3. **Classify contributors by AI usage.** Introduces a caste system: "human-only elite" vs "AI-augmented rest". Cheating just moves to the classification boundary. Every claimed "human" contribution becomes suspicious.

Skilluv takes **option 4**: make AI usage visible and pedagogically framed, without turning it into a merit hierarchy. This RFC codifies option 4 as a stable public commitment, so nobody discovers Skilluv's real position at their first friction moment.

Reason for making it public **now**, six months before launch: partner-project maintainers (RFC 0001) will want to know Skilluv's AI stance before agreeing to a Tier 2 partnership. Ambiguity here blocks partnerships.

## Detailed proposal

### The three zones

Contributions on Skilluv happen in one of three zones. Zone determines rules.

#### Zone 1 — Learning (`unrestricted` or `disclosure_required`)

**Definition**: Working through starter templates, doing tutorial challenges, exploring project code with a mentor, non-competitive personal exploration.

**AI rules**:
- Any tool is welcome.
- Contributors are *invited* (not required) to disclose when a chunk of code came primarily from AI. The purpose is pedagogical dialogue with mentors, not audit.
- Mentors ask "how did you arrive at this?" without penalty — the answer "Claude wrote it and I tweaked" is a valid answer, not a confession.

**Database representation**: `challenge_templates.ai_policy = 'unrestricted'` or `'disclosure_required'`.

#### Zone 2 — Real project (`disclosure_required`)

**Definition**: Contributions to OSS partner projects (Tier 1/2/3), deliverables that ship into a repo maintained by someone else, PRs opened via the `Bonjour Skilluv` flow.

**AI rules**:
- Disclosure is **required** on the deliverable submission form. The `deliverables.ai_disclosure` column stores the disclosure text.
- Disclosure format is *narrative*, not a checkbox. Example: "The initial scaffold was generated with Cursor; the test cases and the docstring were written by me; the refactor of the concurrency section was a back-and-forth with Claude where I refused three of its suggestions." A single-word disclosure ("yes" / "no") is rejected.
- Disclosure quality is reviewed alongside code by the deliverable curator (mentor or Skilluv steward). Poor disclosure is coached, not sanctioned.
- **The upstream project's own policy takes precedence.** If sqlx's `CONTRIBUTING.md` forbids AI-generated code, the Skilluv contributor complies with sqlx's rule, and Skilluv's own policy defers. This is documented per-project in `projects.skilluv_editorial_notes`.

**Database representation**: `challenge_templates.ai_policy = 'disclosure_required'` (default).

#### Zone 3 — Tournament (`human_verified`)

**Definition**: Season-closing capstone challenges (see the S1 and S2 capstones seeded in this repo), Grande Épreuve events, publicly-adjudicated tournaments.

**AI rules**:
- Contributions must undergo a **human verification step** at submission.
- Verification means: a Skilluv mentor watches the contributor walk through the code live (video call, in-person session, or recorded live session with mentor watermark). The mentor asks probing questions about design decisions, alternatives considered, and how the contributor would extend the code.
- The **outcome** of verification is stored in `deliverables.human_verified_at` (timestamp) and `deliverables.human_verified_by` (user_id). No further granularity — either the verification happened or it didn't.
- AI tools remain **welcome** during the tournament's build phase. What tournaments test is not "did the contributor use AI" but "does the contributor understand what shipped".

**Database representation**: `challenge_templates.ai_policy = 'human_verified'` + `challenge_templates.is_capstone = true`.

#### Two edge cases

- **`no_ai_declared`** — a contributor voluntarily commits to no AI usage on this deliverable. Skilluv does not verify this claim (that would require surveillance). It is displayed as-is on the deliverable page, contextualised by disclosure narrative. Not a badge.
- **`ai_native`** — a challenge explicitly designed *around* AI collaboration, where the deliverable is a prompt sequence + evaluation of AI outputs. Rare, not a default, used for specific "learning to work with AI" tracks.

### The absolute non-negotiable: no public classification

Even given the disclosure infrastructure above, **Skilluv commits to the following**:

1. **No AI-usage badges on user profiles.** Not "verified human", not "AI-augmented", not "AI-native". Zero.
2. **No leaderboards split by AI usage.** All contributors compete on the same board, regardless of their AI disclosures.
3. **No search filter allowing enterprises to hide AI-assisted contributors.** Recruiter-facing search does not expose AI disclosure fields.
4. **No AI-usage columns in public data exports.** Disclosure is visible on the specific deliverable page it applies to. It is not aggregated at the profile level.

The rationale is elementary: any classification produces a hierarchy, and any hierarchy produces cheating on the classification. Skilluv does not want to be the platform that trains contributors to lie about AI use.

The founder's original framing, verbatim: *"un badge visible = classification = hiérarchie perçue"*. This RFC promotes that intuition into a binding rule.

### Mentors and AI

Mentors reviewing contributor code may use AI to inform their feedback. If they do, the mentor disclosure appears alongside their review (`reviews.ai_disclosure`, to be added in a follow-up migration). Same standard as contributors: narrative disclosure, no checkboxes.

Mentors are also encouraged to **run AI tools during live mentoring** to model the practice — this is the compagnonnage philosophy applied to AI as a tool.

### AI in the admin panel and platform code

Skilluv's own codebase uses AI assistance (Claude, Copilot). This RFC does not require the Skilluv core team to disclose their AI usage on each commit — code review handles this dimension. However, PRs to `skilluv-community/*` repos (this repo included) **do** carry an AI disclosure line in the PR description when AI was materially involved in drafting. Example: this RFC's Motivation section was drafted iteratively with Claude, edited by @jeremie-zitti.

### Partner-project overrides

Any Tier 2 or Tier 3 partner project may formally request Skilluv route its contributors under stricter rules (up to and including "no AI usage claimed"). The request is honored by adding a `skilluv_editorial_notes` mention on the project record and gating deliverable submission with a project-specific disclosure form.

Skilluv itself does **not** initiate stricter rules on behalf of a project. Silent tightening would break the neutrality promise made to contributors.

## Alternatives considered

**Alternative A — Ban AI entirely.**

Rejected: unenforceable at scale, drives cheating underground, breaks trust the day the first top contributor is caught. Also incoherent with the Skilluv-team-uses-AI reality.

**Alternative B — Endorse AI without disclosure.**

Rejected: skill claims become meaningless. Cal.com or sqlx maintainers will not accept Tier 2 partnership under this regime.

**Alternative C — Three-tier AI badge system on profiles (Verified Human / Assisted / AI-Native).**

Rejected. This was proposed inside Skilluv's founding conversations and firmly refused by the founder: *"peu importe ce qu'on écrit dans la charte, un badge visible = classification = hiérarchie perçue"*. See "no public classification" section above.

**Alternative D — Disclosure yes, but only on capstones.**

Rejected: partial disclosure creates a dishonest signal on the majority of deliverables. If disclosure is worth doing, it is worth doing on every project-shipping deliverable (Zone 2).

**Alternative E — Doing nothing (current state: policy in strategy doc, opaque publicly).**

Rejected. See Motivation. Publishing now settles friction that would otherwise emerge on day 1 of the first cohort.

## Impact

**Impact on contributors**: Neutral to positive. Disclosure narrative is added workload (~1-2 min per deliverable), but it also legitimises their AI use. No penalty for disclosing.

**Impact on mentors**: Positive. Mentors gain a structured entry point to talk with mentees about their process — "walk me through the disclosure" is a warm pedagogical opening, not an accusation.

**Impact on enterprises (hiring side)**: Neutral. Enterprises see disclosure at the deliverable level when they browse a candidate's contributions. They do not see it aggregated. This means they cannot mechanically filter for "human-only" contributors — which is intentional and non-negotiable.

**Impact on partner projects (Tier 1/2/3)**: Positive. Skilluv's stance is explicit, so maintainers can accept or decline Tier 2 partnership on informed grounds. Overrides mechanism accommodates strict maintainers.

**Impact on the codebase**:
- `challenge_templates.ai_policy` enum already implements the 5 modes (migration 0044 + updates).
- `deliverables.ai_disclosure TEXT` already exists (migration 0110).
- Follow-up work: `reviews.ai_disclosure` column, admin UI for submitting narrative disclosure, mentor-view widget for disclosure text alongside code.
- Follow-up work: enterprise-facing search must **not** expose `deliverables.ai_disclosure` as a filter. This is a search implementation constraint tracked in a follow-up technical RFC.

**Impact on ongoing work**: All 20 seeded deliverables (S1 + S2) inherit `ai_policy = 'disclosure_required'` (default). The S1 and S2 capstones explicitly use `human_verified`. No re-seeding needed.

## Open questions

1. **Length of the disclosure narrative.** Should there be a minimum character count, or does that just game the writer? Proposed: no minimum, but the deliverable review guide instructs curators to reject one-word disclosures with coaching feedback.

2. **AI usage inside human verification sessions themselves.** If a contributor uses Claude live during a Zone 3 verification call to answer a mentor's question, does that invalidate the verification? Proposed: allowed, and expected — the mentor's job is to test *understanding*, and modeling AI use during understanding is fine.

3. **Retroactive disclosure edits.** Contributors sometimes realise later that their disclosure was incomplete. Can they edit a submitted disclosure? Proposed: yes, but the edit history is preserved and visible (like git history) — no silent revisions.

4. **Third-party AI training rights.** If AI companies scrape Skilluv contributions for training, does the disclosure allow contributors to opt out? Proposed: this is a separate RFC (data / IP), not this one. Flag for RFC 0004 or later.

5. **Automated tools that hint at AI usage.** GPTZero-style detectors exist. This RFC does not use them. Reason: they are unreliable and their use would immediately restore the surveillance dynamic. But contributor community may raise this as an ask — the answer stays "no" unless a very compelling new argument emerges.

## Rationale

_This section will be filled in after the discussion window closes._

---

## Post-decision

**Decision date**: [pending]
**Decision by**: [pending]
**Result**: [pending]

**Implementation tracking**: [pending — will link to follow-up migrations for `reviews.ai_disclosure` and to admin UI PR for disclosure editing widget]
