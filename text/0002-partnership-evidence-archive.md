# RFC 0002 — Partnership Evidence Archive: Format and Location

- **Date**: 2026-07-22
- **Author(s)**: @jeremie-zitti
- **Status**: Draft
- **Discussion window**: 21 days
- **Related**: RFC 0001 (OSS Partnership Tiers)

## Summary

RFC 0001 requires linkable evidence to promote a project between partnership tiers, but does not specify where or how that evidence is archived. This RFC proposes a **dedicated public repository** `skilluv-community/evidence` with per-project subdirectories containing immutable, timestamped Markdown records for each tier event.

The design goals are: **immutability** (no rewriting history on demotion), **linkability** (URLs stable for years), **human-browsability** (someone auditing sqlx's Tier 2 status finds the record in one click), and **machine-readability** (front-matter parseable by the platform).

## Motivation

Without an archive rule, evidence for tier promotions will accumulate in ad-hoc places: a GitHub thread here, a screenshot there, a Google Doc that later 404s. When a maintainer three years from now asks "why is my project listed as Tier 3?" or when a Skilluv contributor asks "what does Tier 2 actually mean for sqlx?", the answer must be one URL away.

The cost of not solving this now is that the tier system loses credibility as soon as the first URL rots. And URLs *will* rot — Twitter threads disappear, comment IDs get renumbered on migrations, screenshots get lost.

Skilluv publishes evidence in its own space, under its own control, with a format that outlasts individual platforms.

## Detailed proposal

### Repository

Create `skilluv-community/evidence`.

- **License**: CC BY-SA 4.0 (same as changelog and rfcs).
- **README**: explains the archive purpose and points to this RFC.
- **Directory structure**:

```
skilluv-community/evidence/
├── README.md
├── LICENSE
├── projects/
│   ├── sqlx/
│   │   ├── 2027-04-15-tier-2-promotion.md
│   │   ├── 2027-11-03-tier-3-mou-summary.md
│   │   └── 2028-04-15-annual-audit.md
│   ├── calcom/
│   │   └── ...
│   └── ...
└── template.md
```

The `projects/{slug}/` directory name **must exactly match** the `projects.slug` value in the Skilluv database. This makes the URL predictable: `https://github.com/skilluv-community/evidence/tree/main/projects/sqlx`.

### Record format

Each record is a single Markdown file, filename `YYYY-MM-DD-{event-slug}.md`. The event slug describes the type of record:

- `tier-2-promotion` — first promotion to Tier 2
- `tier-3-mou-summary` — public summary of a signed MoU (never the MoU itself)
- `annual-audit` — the yearly proof-retention audit (RFC 0001 open question #3)
- `demotion-{tier-N}` — a tier demotion event with rationale
- `withdrawal` — a maintainer-requested removal from the catalogue

### Front-matter schema

```markdown
---
project_slug: sqlx
event_type: tier-2-promotion
event_date: 2027-04-15
tier_before: 1
tier_after: 2
decision_by: [jeremie-zitti]
rfc_reference: 0001
proofs:
  - kind: outreach_thread
    url: https://github.com/launchbadge/sqlx/discussions/1234#discussioncomment-5678
    archived_at: https://web.archive.org/web/2027*/...
  - kind: merged_contribution
    url: https://github.com/launchbadge/sqlx/pull/1500
    contributor: skilluv-user-{uuid}
  - kind: maintainer_acknowledgement
    url: https://github.com/launchbadge/sqlx/discussions/1234#discussioncomment-9999
    maintainer_handle: abonander
    archived_at: https://web.archive.org/web/2027*/...
skilluv_admin_decision_id: 42
---

# sqlx → Tier 2 Promotion (2027-04-15)

[Human-readable narrative...]
```

- Every URL in `proofs.url` **must** be accompanied by an `archived_at` field
  pointing to a Wayback Machine snapshot. This is the immutability guarantee
  even when the origin thread disappears.
- `skilluv_admin_decision_id` links back to the internal audit trail on the
  admin panel (`services::audit::record` row id). This closes the loop:
  the public record maps to an immutable internal audit event.

### Immutability rules

- **A published record is never edited**, only supplemented by a newer record
  in the same directory (e.g., a `demotion-tier-1.md` file follows a
  `tier-2-promotion.md` — the promotion stays visible, historically).
- **Typo corrections** are the only allowed edits, and must be committed as a
  separate `git commit` with an explicit message pattern:
  `docs(evidence): typo — {original-filename} — {word}`.
- **Renames are forbidden** — the filename is part of the immutable identity.
- **Deletions are forbidden** except in two cases:
  1. Legal takedown (with a public statement in the same PR).
  2. Maintainer withdrawal request that specifies removal of prior records
     as part of the withdrawal terms.

Both allowed deletions require a `withdrawal.md` record documenting the deletion.

### Integration with the platform

The Skilluv admin CRUD `/admin/projects` PATCH endpoint (skilluv-backend
`admin_projects.rs`) is extended to require an evidence-URL when promoting
`skilluv_partnership_level` from 1→2 or 2→3.

- **Input validation**: the URL must match
  `^https://github\.com/skilluv-community/evidence/blob/main/projects/{slug}/\d{4}-\d{2}-\d{2}-.+\.md$`
  where `{slug}` equals the project's slug.
- **Runtime verification**: the endpoint performs a `GET` on the URL,
  parses the front-matter, and verifies `project_slug`, `event_type`, and
  `tier_after` match the intended promotion.
- If verification fails, the promotion is refused with a clear 422 response.

Corresponding admin UI change: the tier promotion form gains an "Evidence
URL" field, required when transitioning up, disabled when transitioning
down (demotions link to the evidence via a follow-up form step).

### Frontend rendering

Public project pages display the tier badge (see RFC 0001) as a **link**
to the most recent promotion/audit record for that project. This is the
one-click rule: any visitor confused by a tier claim gets to the evidence
without asking anyone.

### Bootstrapping

- On merge of this RFC, `skilluv-community/evidence` is created with the
  README, LICENSE, `template.md`, and empty `projects/`.
- The first record ever committed to the archive is
  `projects/_meta/2026-07-22-archive-inception.md` documenting that the
  archive was born on this date under this RFC.
- Existing Tier 1 projects require **no evidence records**. Tier 1 means
  editorial curation only — no external proof is claimed.

## Alternatives considered

**Alternative A — Evidence lives in `skilluv-community/rfcs` as amendments to RFC 0001.**

Rejected: RFCs are documents about *decisions*, not archives of *events*.
Mixing them makes RFC 0001 grow unboundedly and makes discovery harder.

**Alternative B — Evidence lives in the `skilluv-community/changelog` weeklies.**

Rejected: weeklies are chronological cross-cuts across the platform.
Finding sqlx's specific promotion history means grepping through 100+
weeklies. Wrong index.

**Alternative C — Evidence lives inline in `projects.skilluv_editorial_notes`
in the database, with the admin panel rendering it as a public page.**

Rejected: database rows lose their history when edited. Migration bugs, admin
UI mistakes, or a rogue admin session could rewrite past evidence silently.
Git commits are the only immutability primitive we already trust.

**Alternative D — No archive; rely on the URLs in `skilluv_editorial_notes`
being kept up to date manually.**

Rejected as unstable — see Motivation. This is the current state, and it's
the state the RFC exists to replace.

**Alternative E — A separate repo per project.**

Rejected as over-engineering. 12 projects today, plausibly 50 in 3 years —
50 GitHub repos of ~5 files each is a discovery mess. One repo with
per-project directories keeps the ratio right.

## Impact

**Impact on the platform code**:
- `skilluv-backend/src/routes/admin_projects.rs` — extend PATCH handler to
  require + verify evidence URL on tier promotion.
- `skilluv-admin/src/routes/projects/+page.svelte` — add "Evidence URL"
  field to the tier promotion form.
- `skilluv-frontend` — tier badges become links to the most recent evidence
  record for the project.

**Impact on maintainers**: Positive — maintainers whose projects are
promoted always find a public record of the promotion's justification, with
their own acknowledgement quoted verbatim.

**Impact on contributors**: Positive — a Skilluv contributor whose merged
PR triggered a Tier 2 promotion is publicly credited on the evidence record.
This is a real portfolio artifact.

**Impact on ongoing work**:
- No existing Tier 2/3 projects to migrate — everyone is Tier 1 per RFC 0001.
- The 12 curated projects at Tier 1 need **no** evidence records.
- The next actual promotion (whenever that happens) is the first record.

**Storage / cost impact**: negligible. Text-only records, no attachments.

## Open questions

1. **Wayback Machine dependency**. Requiring an `archived_at` field assumes
   web.archive.org stays free and accessible. Is there a self-hosted archive
   fallback we should specify? (Proposal: a script in the evidence repo that
   periodically re-snapshots evidence URLs and stores hashes.)

2. **Contributor credit removal request**. If a Skilluv contributor whose PR
   triggered a promotion later asks to be removed from the record (e.g.,
   privacy concerns), do we replace their name with `[redacted-by-request]`
   or do we delete the entire record? First option lighter, second option
   preserves the promotion but loses attribution.

3. **Machine-readable index**. Should the evidence repo also expose a
   `_index.json` regenerated on each commit, so the frontend can query
   evidence without cloning the full repo? Not needed day-1, but the
   presence of front-matter makes it trivial to add later.

4. **Cross-language records**. RFC 0001 was written EN-primary, per
   community charter policy. Should evidence records also default to EN,
   or should they be written in the language of the primary maintainer
   acknowledgement (which might be French, German, etc.)? Proposal: EN
   default, but allow FR / EN duplication for maintainers who acknowledged
   in French.

## Rationale

_This section will be filled in after the discussion window closes._

---

## Post-decision

**Decision date**: [pending]
**Decision by**: [pending]
**Result**: [pending]

**Implementation tracking**: [pending — will link to skilluv-backend PR + skilluv-admin PR + skilluv-community/evidence repo creation]
