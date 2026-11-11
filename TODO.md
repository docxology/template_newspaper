# template_newspaper TODO

This backlog is future-only. Completed validation and dated review evidence are preserved in
[`docs/maintenance/exemplar-backlog-history.md`](../../../docs/maintenance/exemplar-backlog-history.md)
or in source-owned generated receipts. Each active row must retain a stable ID, size, dependency,
next action, proving artifact, acceptance command, and negative control; absence of an owner or external receipt
keeps a capability blocked rather than silently promoting it.

## Backlog operating rules

- Keep deterministic and offline defaults unchanged unless an upcoming row explicitly scopes an opt-in.
- Do not close a row until its producer, artifact, consumer, gate, and failing negative control are present.
- Treat unavailable network, LLM, container, formal-tool, and publication paths as explicit skips
  or blockers.
- Re-derive counts and receipts from live source data; never copy measurements into this planning file.

## Integrity and template-status gaps

- Keep edition content fictional unless a fork adds real source provenance and fact-checking validators.
- Keep ReportLab layout logic in `src/`, with scripts as thin orchestration only.
- The machine-readable `layout_audit` artifact records page geometry and
  glyph-clearance findings separately from `all_pages_fit`, overset, and
  missing-image checks; keep all four signals distinct.

## Configurable-surface gaps

- Keep `manuscript/config.yaml.example` aligned with publication metadata and render toggles.
- A minimal one-page fork schema example is required in the same row if
  `content/edition.yaml` ever gains required fields.

## Documentation and signposting gaps

- Keep README and docs clear that the newspaper PDF is produced by project scripts, while the manuscript PDF is produced by the monorepo renderer.
- Link any new content schema fields from `docs/syntax_guide.md` and the README quick-start.
- The platform-dependent `typography.py` `register_fonts()` fallback arc is documented in the README, architecture, and testing guide; its base-14 branch remains uncovered under the no-mocks policy.

## Current test and validator contract

- Documentation-only README numbers are excluded from empirical claims, and
  `infrastructure.core.pipeline.artifacts.snapshot_current_artifact_manifest`
  is the stable single-stage refresh path.
- Keep the platform-only `typography.py` fallback branch documented rather than mock-covered; revisit only if the no-mocks policy or the CI font matrix changes.

## Minor upcoming

| ID | Status | Size | Dependency | Next action / unblock condition | Proving artifact | Acceptance command | Negative control |
| --- | --- | --- | --- | --- | --- | --- | --- |
No active rows are currently scoped at this size.

## Medium upcoming

| ID | Status | Size | Dependency | Next action / unblock condition | Proving artifact | Acceptance command | Negative control |
| --- | --- | --- | --- | --- | --- | --- | --- |
No active rows are currently scoped at this size.

## Major upcoming

| ID | Status | Size | Dependency | Next action / unblock condition | Proving artifact | Acceptance command | Negative control |
| --- | --- | --- | --- | --- | --- | --- | --- |
No active rows are currently scoped at this size.

## Backlog status

Rows remain active until the acceptance command and negative control pass in the same source revision.
A blocked row is a deliberate boundary, not a skipped success.

## Workflowz review — 2026-11-10

- [ ] **WFZ-20261110-01 — P1 — Reject an invalid enabled spot color instead of silently rendering without it.** Outcome: when `render.spot_color` is true and `render.spot_hex` is malformed, loading or rendering the edition fails with an actionable configuration error naming `spot_hex`; valid hex colors still produce a ReportLab color and disabled spot color still returns `None`. Evidence: `src/newspaper/config.py:107-117` catches every exception from `HexColor` and returns `None`, while `src/newspaper/engine.py:92` passes that result into rendering. Acceptance probe from repository root: `uv run python -c 'from newspaper.config import NewspaperConfig; import sys; sys.path.insert(0, "src"); c=NewspaperConfig(spot_color=True, spot_hex="not-a-color"); c.spot()'` must exit nonzero with a clear `spot_hex` validation message (place `sys.path.insert` before the import in the actual probe); then `uv run python -c 'import sys; sys.path.insert(0, "src"); from newspaper.config import NewspaperConfig; assert NewspaperConfig(spot_color=True, spot_hex="#9E1B32").spot() is not None; assert NewspaperConfig(spot_color=False, spot_hex="not-a-color").spot() is None'` must succeed. Dependencies/constraints: none beyond existing PyYAML/ReportLab; preserve the explicit disabled-spot behavior and do not change deterministic rendering defaults. Add a regression test alongside the implementation, with the malformed enabled value as the negative control.

- [ ] **WFZ-20261110-02 — P1 — Parse render booleans strictly rather than coercing arbitrary strings.** Outcome: YAML booleans retain their intended values, and quoted non-boolean values for `draft_grid` or `spot_color` raise a clear `ValueError` instead of being truthiness-coerced. Evidence: `src/newspaper/config.py:102-103` applies `bool(...)` directly to values returned by `yaml.safe_load` at `src/newspaper/config.py:125`; consequently a quoted value such as `"false"` becomes `True`. Acceptance probe from repository root: `uv run python -c 'import sys; sys.path.insert(0, "src"); from newspaper.config import NewspaperConfig; assert NewspaperConfig.from_dict({"render": {"draft_grid": False, "spot_color": False}}).draft_grid is False; NewspaperConfig.from_dict({"render": {"spot_color": "false"}})'` must exit nonzero specifically for the quoted invalid value, while `uv run python -c 'import sys; sys.path.insert(0, "src"); from newspaper.config import NewspaperConfig; c=NewspaperConfig.from_dict({"render": {"draft_grid": True, "spot_color": False}}); assert c.draft_grid is True and c.spot_color is False'` must succeed. Dependencies/constraints: no new dependency; preserve support for actual YAML boolean values and the existing strict-unknown-key contract. Cover both fields with behavioral tests and use quoted `"false"` as the failing negative control.
