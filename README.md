# ai-pr-collision-lab

> **Historical hosted demo.** The project-hosted Veripsa GitHub App has been
> suspended since **2026-08-18**. Installing the App will not currently produce
> checks or comments. The recording below documents the historical workflow.
> You can still run this fixture locally; for local signal
> evaluation, see [Veripsa Core offline evaluation](https://github.com/RollNuts/veripsa#offline-evaluation).

A small Python order-pricing service used for a historical Veripsa demo of two
pull requests that overlap in the same pricing code. The recording shows the
past GitHub check and comment; local tests let you inspect the fixture today.

## Start here

| Need | Link |
| --- | --- |
| Run the pricing tests | [Run locally](#run-locally) |
| Reproduce the prepared scenarios | [Current canonical fixture guidance](#reproduce-both-scenarios) |
| Historical check and comment recording | [Recorded public run](#recorded-public-run) |
| This fork's legacy fixture manifest | [`demo-manifest.json`](./demo-manifest.json) |
| Public integration contract v1.0.0 | <https://github.com/GetVeripsa/veripsa-webhook-spec/tree/v1.0.0> |
| Data handling and retention | [`DATA_HANDLING.md`](./DATA_HANDLING.md) |

## What is in this repository

- `orders/pricing.py` — order totals; both prepared scenarios edit it.
- `orders/catalog.py` — a tiny in-memory product catalog.
- `tests/test_pricing.py` — plain pytest tests.
- `demo-manifest.json` — the machine-readable authority for fixture branches,
  their shared base, expected changed paths, tests, recorded-run metadata, and
  compatible public-contract version.
- `scripts/verify_fixtures.py` — the legacy verifier for the scenario branches;
  those branch refs are not included in this fork.

Two historical scenario branches (see [current reproduction guidance](#reproduce-both-scenarios)):

| Branch | Pretend author | Purpose |
| --- | --- | --- |
| `collide-a` | Agent A | Add a bulk discount inside calculate_total. |
| `collide-b` | Agent B | Add a large-cart discount in the same pricing area. |

Each branch is small and passes its tests independently. Both must keep the
recorded fixture base
`03f70bef401b65e0c275c8b34cf9fc6d83f85379`, and each must differ from it only
in `orders/pricing.py` and `tests/test_pricing.py`. The legacy verifier checks those invariants when both branch refs are present;
README prose is not the fixture authority.

## Recorded public run

![A Veripsa comment shows two open PRs, then updates after the leading PR merges](./media/veripsa-two-prs-before-after.gif)

The real public run was captured on **2026-07-16** at commit
[`03f70bef401b65e0c275c8b34cf9fc6d83f85379`](https://github.com/GetVeripsa/ai-pr-collision-lab/commit/03f70bef401b65e0c275c8b34cf9fc6d83f85379),
compatible with public contract **v1.0.0**:

- [PR #1](https://github.com/GetVeripsa/ai-pr-collision-lab/pull/1) held the
  earlier position while PR #2 was open.
- [PR #2](https://github.com/GetVeripsa/ai-pr-collision-lab/pull/2) updated after
  the earlier PR landed.
- A compact [MP4 version](./media/veripsa-two-prs-before-after.mp4) is included
  for social posts and presentations.
- Immutable media links: [GIF](https://github.com/GetVeripsa/ai-pr-collision-lab/raw/03f70bef401b65e0c275c8b34cf9fc6d83f85379/media/veripsa-two-prs-before-after.gif)
  and [MP4](https://github.com/GetVeripsa/ai-pr-collision-lab/raw/03f70bef401b65e0c275c8b34cf9fc6d83f85379/media/veripsa-two-prs-before-after.mp4).

This is a historical capture of the visible GitHub workflow, not a frozen copy
contract. Human sentence copy may evolve. Integrations should consume the
current machine contract instead of parsing prose from the recording. The
capture does not assert that either change is correct or safe to merge.

## Run locally

This is a historical fork. Its `main` branch has the small pricing service and
unit tests, but does not contain the scenario branches or the newer fixture
materializer. To run the service tests without installing a GitHub App:

```sh
python3 -m venv .venv
. .venv/bin/activate
python3 -m pip install -e '.[test]'
python3 -m pytest -q
python3 scripts/check_docs.py
```

The pricing tests exercise the local Python fixture. They do not produce a
Veripsa check or validate the hosted service.

## Reproduce both scenarios

The canonical repository has moved from long-lived `collide-a` / `collide-b`
branches to the annotated tags `fixture-v1-agent-a` / `fixture-v1-agent-b`.
Its newer manifest and scripts verify the tags and materialize scenario branches
in a fork you control. Use the
[canonical scenario instructions](https://github.com/GetVeripsa/ai-pr-collision-lab#the-two-prepared-scenarios)
and [materializer](https://github.com/GetVeripsa/ai-pr-collision-lab/blob/main/scripts/materialize_fixtures.py)
from a fresh clone of that repository. The legacy `scripts/verify_fixtures.py`
in this fork expects the old branch names and cannot verify scenarios that are
absent here. Do not delete a fork to try to restore those removed branches.

The hosted installation and check-rendering steps in older walkthroughs are
historical while the service remains suspended. Local fixture tests and tag
verification remain useful without an App installation. For evaluating the
structural signal on your own repository, use
[Veripsa Core offline evaluation](https://github.com/RollNuts/veripsa#offline-evaluation).

Forking, cloning, and testing this static repository sends nothing to Veripsa.
[`DATA_HANDLING.md`](./DATA_HANDLING.md) documents the historical App data
boundary; it is not an availability notice for a hosted service.

## License

The code and documentation in this lab are available under the
[MIT License](./LICENSE).
