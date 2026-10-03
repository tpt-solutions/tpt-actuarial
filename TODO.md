# tpt-actuarial: TODO

Owner: TPT Solutions · License: **MIT OR Apache-2.0** (every crate; Apache-2.0-only deps are disqualifying)
Status: `needs-scoping` (see spec.txt). Phase 0 gates everything below it.
Build order: mortality → triangle → credibility → reserve → cashflow → pricing → umbrella

Legend: `[ ]` todo · `[x]` done · 🔒 blocked by Phase 0

---

## Phase 0: Prior-art gate (blocks promotion to `planned`)

- [ ] Confirm upstream locations (local paths + GitHub URLs): tpt-math-*, tpt-eng-timeseries, tpt-rust-map, tpt-vertical-map, tpt-erp-rust
- [ ] Read tpt-rust-map/registry.toml `[[audit]]` list; confirm no existing tpt-solutions repo covers actuarial ground
- [ ] Read this repo's registry.toml `tpt-actuarial` `[[vertical]]` entry
- [ ] Read tpt-vertical-map/spec.txt CORE PRINCIPLES
- [ ] Verify each needed primitive exists (or is flagged/planned) in a tpt-rust-map pillar:
  - [ ] tpt-math-prob-core (Distribution trait, MLE, conjugate pairs)
  - [ ] tpt-math-prob-dist (Gompertz, Makeham, ODP, log-normal)
  - [ ] tpt-math-prob-monte-carlo
  - [ ] tpt-math-prob-bayes
  - [ ] tpt-math-stats (GLM, variance/covariance, chi-squared)
  - [ ] tpt-math-optimize-general
  - [ ] tpt-math-units
  - [ ] tpt-eng-timeseries
- [ ] Audit tpt-erp-rust event-sourced ledger and audit-trail patterns; decide what to reuse for the audit trail
- [ ] Read rslife source (MIT): does it cover select-and-ultimate and multi-state? Decide: wrap / absorb / inspiration only
- [ ] Investigate kestrel (license + content); discard if still a stub
- [ ] Decide triangle storage: ndarray vs polars (spec leans ndarray for triangle, polars for pricing)
- [ ] Check every external dependency license (cargo-deny): MIT or MIT OR Apache-2.0 only
- [ ] Update registry.toml `status = "needs-scoping"` → `planned`
- [ ] Finalize CRATE INVENTORY in spec.txt

## Phase 1: Repo foundation 🔒

- [ ] Seed from tpt-vertical-map/template/
- [ ] Cargo workspace with crates under `crates/` (edition 2021, stable)
- [ ] `LICENSE-MIT` and `LICENSE-APACHE` ("Copyright (c) <year> TPT Solutions")
- [ ] `license = "MIT OR Apache-2.0"` in every Cargo.toml
- [ ] README, CONTRIBUTING, CHANGELOG, SECURITY
- [ ] Register every crate in registry.toml **before** implementing it
- [ ] Path deps for local dev; git/GitHub URLs for CI/publish
- [ ] GitHub Actions: fmt, clippy, test
- [ ] cargo-deny: reject Apache-only and other non-compliant licenses
- [ ] `no_std + alloc` build check for mortality, triangle, credibility
- [ ] Cross-OS determinism CI (identical output on Linux/macOS/Windows)
- [ ] Golden-test fixture layout (R reference outputs + published examples, with provenance notes)
- [ ] Seeded RNG convention for all stochastic code

## Phase 2: tpt-actuarial-mortality (`no_std + alloc`) 🔒

- [ ] Crate skeleton, registered
- [ ] Unit types for ages, force of mortality, effective rates (tpt-math-units)
- [ ] Aggregate tables (attained age)
- [ ] Select-and-ultimate tables (select age × duration)
- [ ] Fractional-age interpolation: UDD, constant force, Balducci
- [ ] Gompertz–Makeham parametric law + MLE fitting
- [ ] Improvement scales (e.g. MP-2021)
- [ ] Multi-state models (alive/disabled/dead; able/unable/dead)
- [ ] Curtate and complete future lifetime
- [ ] Annuity PVs: immediate, due, deferred, n-year certain and life
- [ ] Insurance PVs: term, whole life, endowment
- [ ] Route reusable distributions (Makeham etc.) upstream to tpt-math-prob-dist
- [ ] Golden tests vs R lifecontingencies and textbook examples
- [ ] Docs + methodology note

## Phase 3: tpt-actuarial-triangle (`no_std + alloc`) 🔒

- [ ] Crate skeleton, registered
- [ ] Triangle type (accident year × development year; cumulative and incremental; paid/incurred)
- [ ] Development factor selection: volume-weighted, simple average, selected LDF
- [ ] Chain ladder
- [ ] Bornhuetter-Ferguson
- [ ] Cape Cod
- [ ] Clark LDF and Clark Cape Cod (MLE, growth curves)
- [ ] Tail factor selection and extrapolation
- [ ] Time indexing via tpt-eng-timeseries
- [ ] Golden tests vs R ChainLadder and published triangles
- [ ] Docs + methodology note

## Phase 4: tpt-actuarial-credibility (`no_std + alloc`) 🔒

- [ ] Crate skeleton, registered
- [ ] Limited fluctuation (full/partial)
- [ ] Bühlmann (Model 1)
- [ ] Bühlmann-Straub (Model 2)
- [ ] Greatest-accuracy / Bayesian with conjugate priors
- [ ] Golden tests vs textbook examples
- [ ] Docs + methodology note

## Phase 5: tpt-actuarial-reserve (`std`) 🔒

- [ ] Crate skeleton, registered
- [ ] Mack chain ladder (process and parameter variance)
- [ ] ODP bootstrap (England-Verrall, Pearson residual resampling, seeded)
- [ ] Merz-Wüthrich one-year reserve risk (CDR)
- [ ] Reserve range outputs: point estimate, standard error, VaR 75/95/99.5
- [ ] Residual diagnostics (chi-squared)
- [ ] Bit-identical results with fixed seed, tested across platforms
- [ ] Golden tests vs Mack (1993), England-Verrall, R ChainLadder
- [ ] Docs + methodology note

## Phase 6: tpt-actuarial-cashflow (`std`) 🔒

- [ ] Crate skeleton, registered
- [ ] Discount curves: Smith-Wilson / UFR extrapolation (EIOPA), Treasury bootstrapping
- [ ] IFRS 17 GMM: fulfilment cash flows, risk adjustment, CSM accretion and roll-forward
- [ ] Onerous contracts: loss component tracking
- [ ] Solvency II BEL (probability-weighted PV on EIOPA risk-free curve)
- [ ] Solvency II risk margin (6% cost of capital × projected SCR)
- [ ] Stochastic scenario generation for Internal Model BEL
- [ ] Golden tests vs published EIOPA curves
- [ ] Flag VFA and PAA as v2 scope (document, don't build)
- [ ] Docs + methodology note

## Phase 7: tpt-actuarial-pricing (`std`) 🔒

- [ ] Crate skeleton, registered
- [ ] Rating plan structure (base rate + relativities, multiplicative/additive)
- [ ] Loss ratio method
- [ ] Pure premium method
- [ ] Indicated rate change (development + trend + expenses)
- [ ] GLM-based relativities (via tpt-math-stats)
- [ ] Credibility-weighted experience (uses credibility crate)
- [ ] Frequency/severity distribution fitting
- [ ] Decide: GBM relativities here vs separate `tpt-actuarial-ml` (deferred until a real consumer)
- [ ] Docs + methodology note

## Phase 8: tpt-actuarial umbrella 🔒

- [ ] Re-export all six crates behind named Cargo features
- [ ] Feature-matrix CI (each feature alone, all together, none)
- [ ] Top-level docs and examples

## Phase 9: Extension scope (not in spec.txt)

- [ ] Confirm these are wanted once libraries stabilise
- [ ] `tpt-actuarial-cli`: CSV/JSON in, reproducible reports out, single static binary
- [ ] Python bindings (PyO3): wheels, type stubs, R/pandas-friendly I/O
- [ ] Register both in registry.toml before starting

## Phase 10: Cross-cutting

- [ ] Review: every calculation is a pure function of typed inputs → typed outputs
- [ ] Audit-trail / input-traceability design (informed by tpt-erp-rust audit)
- [ ] Per-crate methodology docs a regulator could review (Solvency II / IFRS 17)
- [ ] rustdoc coverage + runnable examples
- [ ] Benchmarks (stochastic scenario throughput vs R)
- [ ] Security / dependency audit (cargo-audit)
- [ ] crates.io publish in build order; version policy
- [ ] Release checklist and announcement

## Out of scope (per spec)

- Policy administration, premium billing, claims workflow → tpt-erp-rust / separate vertical
- Core distributions and statistical estimation → tpt-math-prob-core, tpt-math-stats
