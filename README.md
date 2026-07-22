# Skilluv RFCs (Request for Comments)

Public tracker for structuring decisions on Skilluv.

## What is an RFC?

A Request for Comments is a structured document proposing a significant change to Skilluv — product, technology, governance, community process. Inspired by [Rust RFCs](https://github.com/rust-lang/rfcs) and [Python PEPs](https://peps.python.org/).

## When to open an RFC

Open an RFC when the change:
- Modifies the Skilluv product roadmap or strategy
- Changes the community charter or governance
- Introduces a new stack technology to the official recommendations
- Modifies the economic model (talents, mentors, enterprises)
- Requires community feedback before implementation

**You do NOT need an RFC for:**
- Bug fixes
- Small UI improvements
- Documentation updates
- Individual contributions to existing repositories

## Process

1. **Draft** — fork this repo, create `NNNN-my-topic.md` in `text/` using [template.md](./text/template.md). Number is next available (check open PRs to avoid collisions).
2. **Open PR** — the RFC PR is the discussion venue.
3. **Discussion window** — 14 days minimum for the community to comment. Some RFCs have longer windows (30 days for structural changes).
4. **Final decision** — by Jérémie in 2027 (see governance in [RFC 0000](./text/0000-governance.md)). By steward committee from 2028.
5. **Merged or rejected** — merged RFC = "Accepted". Rejected RFC = closed with rationale.
6. **Implementation** — accepted RFCs are tracked in a follow-up issue on the impacted repository.

## Statuses

- **Draft** — being written
- **Discussion** — PR open, comments welcome
- **Accepted** — merged, awaiting implementation
- **Rejected** — closed, rationale in final comment
- **Superseded** — replaced by a later RFC
- **Withdrawn** — author gave up

## Existing RFCs

See [`text/`](./text/) for the list. Notable RFCs:

- [RFC 0000 — Governance (meta-RFC)](./text/0000-governance.md)

## Governance

The RFC process is itself defined by [RFC 0000 — Governance](./text/0000-governance.md), which is a **meta-RFC** describing how Skilluv makes decisions.

## License

Contributions to this repository are licensed under [CC BY-SA 4.0](./LICENSE).

## Contact

- Public: `#rfcs` category on the Skilluv forum
- Private: `rfcs@skilluv.io`
