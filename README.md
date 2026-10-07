# Diamond PAU — Independent Formal Assurance and Adversarial Verification

> A 21-property attempt combining Certora, Halmos, Foundry, mutation
> discrimination, historical regression testing, and deployed-bytecode binding.

We performed an independent property-driven assurance campaign against Diamond PAU, including formal verification where tractable, symbolic and concrete production-path analysis, deployed-runtime binding, targeted mutation discrimination, and replay of five historical regression classes.

No live Diamond PAU vulnerability was established within the tested scope. Several properties remain explicitly unresolved because the formal models were not trustworthy or tractable enough to support stronger claims.

This is NOT:

- A claim that Diamond PAU is vulnerability-free.
- A universal formal verification of the system.
- A substitute for future review after code or configuration changes.

## Key results

The final classifications are:

- **4 VERIFIED-WITH-DOCUMENTED-ASSUMPTIONS**
- **2 PARTIALLY-VERIFIED**
- **8 MULTI-ENGINE-ASSURANCE**
- **4 UNKNOWN / BLOCKED-MODELING**
- **1 UNRESOLVED / BLOCKED-MODELING**
- **1 REUSE**
- **1 FIXTURE**

These are assurance classifications, not scores or percentages. The campaign accounted for all **21 manifest entries**: 19 assurance properties, C07 as specification reuse, and G01 as frozen-fixture support. C02's committed primary classification is UNKNOWN; its Certora boundary is BLOCKED-MODELING, placing it in the four-unknown summary group.

The campaign revisited **five historical regression classes**, each with executable evidence and targeted mutation discrimination. Across the campaign, **14 executed targeted mutants were discriminated** where accepted execution evidence existed; planned or unexecuted mutations are not counted. These results do not establish mutation completeness.

Runtime/source reconciliation completed with **zero unexplained executable mismatches** in the accepted evidence. This establishes executable/source-family correspondence under the recorded profile, rather than behavioral correctness or a fresh attestation of every live deployment.

## How to read this repository

Recommended order:

1. [Final Assurance Report](reports/FINAL-ASSURANCE-REPORT.md) — Full campaign synthesis, classifications, engine boundaries, deployment binding, and closeout conclusions.
2. [Property Results](reports/PROPERTY-RESULTS.md) — Human-readable 21-property matrix, with each objective, committed result, evidence, and main limitation.
3. [Historical Regressions](reports/HISTORICAL-REGRESSIONS.md) — Five regression classes, repairs, executable challenges, and differences in mutation fidelity.
4. [Formal Proof Coverage](reports/FORMAL-PROOF-COVERAGE.md) — What was genuinely formally proved, partially proved, bounded, or blocked.
5. [Assumptions and Limits](reports/ASSUMPTIONS-AND-LIMITS.md) — Scope boundaries, property-specific assumptions, economic limits, and nonclaims.

[data/](data/) contains machine-readable structured evidence, including the [property matrix](data/property-matrix.json), [regression register](data/regression-register.json), [formal coverage](data/formal-proof-coverage.json), [unresolved register](data/unresolved-register.json), and [nonclaims](data/nonclaims.json).

[provenance/](provenance/) contains canonical campaign provenance/hash material. These records retain references into the full assurance repository; this minimal export does not contain every referenced artifact. The final synthesis likewise retains internal evidence references. The human-readable summaries provide a reading path within this export.

[PUBLIC-RELEASE-SHA256SUMS.txt](PUBLIC-RELEASE-SHA256SUMS.txt) is the included checksum manifest. Its current path and coverage limitations are explained under integrity verification below.

## What “MULTI-ENGINE-ASSURANCE” means

The engines supply complementary evidence:

- **Forge**, Foundry's test runner, executes concrete production-path and regression scenarios.
- **Halmos** explores symbolic EVM execution over the recorded bounded domains or fixed paths.
- **Certora** supplies theorem-style verification of specified rules within documented models and assumptions where faithful modeling was tractable.
- **Mutation discrimination** checks targeted defect sensitivity by deliberately altering behavior and confirming that intended assertions detect it, alongside usable controls.

For MULTI-ENGINE-ASSURANCE properties, Forge/Halmos support the recorded composed behavior; the classification does not imply a Certora proof. Compilation alone is not verification. Combining engines does not mathematically equal universal formal verification.

C03, C09, C10, and C11 have scoped Certora local/state-transition proofs. C04 and R04 remain partial. R04 particularly illustrates the distinction: its zero-case and represented-threshold minimum theorem does not prove the entire production arithmetic path.

Several scenes used deterministic external endpoints and selected state observations. Their evidence does not establish live protocol economics, arbitrary token behavior, every callback, or an exhaustive storage-frame theorem.

## Historical regressions

The five targets were:

- **REG-001 — UniV3 accrued-fee contamination**
- **REG-002 — Farm direct claim capability**
- **REG-003 — Farm withdrawal/reward bypass**
- **REG-004 — UniV3 boundary/rounding**
- **REG-005 — CCTP fee/chunk accounting**

All five targeted mutants were discriminated by Forge/Halmos; R04's mutant was additionally discriminated by scoped Certora verification. No tested historical regression was established as live in the current scoped behavior.

Fidelity differed: R01 reversed the exact repair hunk, R02/R03 restored historical behavior, R04 used a targeted non-historical threshold mutation, and R05 used a historical-style fee-reuse mutation. They were not five complete reproductions of old implementations. See [Historical Regressions](reports/HISTORICAL-REGRESSIONS.md) for the practical failure modes and residual boundaries.

## Unresolved / modeling-boundary results

**C02, C05, C06, C08, and G02** remain explicit unknowns because the modeling/tooling boundary did not support a trustworthy stronger claim. Their blockers involve low-level call/delegatecall success paths, fallback memory/call tracing, nested configuration returns, or setup/loop/hash behavior. Narrow passing checks do not close these objectives, and failed modeled obligations do not establish reachable production vulnerabilities.

Unresolved subclaims also remain: **R04 amount-delta equivalence**, **R02 Certora correspondence obligations**, and **R05's arithmetic-reference timeout**. C04 retains role-enumeration/removal and arbitrary-state gaps; C09's independent sequential Halmos timeout remains separate from its Certora proofs.

Consult [Formal Proof Coverage](reports/FORMAL-PROOF-COVERAGE.md) and [Assumptions and Limits](reports/ASSUMPTIONS-AND-LIMITS.md) before extending any conclusion beyond its recorded domain. Blocked and timed-out results are neither passes nor established live bugs.

## Integrity / verification

The public release includes [PUBLIC-RELEASE-SHA256SUMS.txt](PUBLIC-RELEASE-SHA256SUMS.txt). From the repository root, the checksum verification command is:

```sh
shasum -a 256 -c PUBLIC-RELEASE-SHA256SUMS.txt
```

**Current manifest limitation:** the included manifest lists ten `final/` paths that are absent from this minimal export; the structured registers are published under `data/`, with the campaign ledger under `provenance/`. Its listed synthesis file under `evidence/` verifies successfully, but the command reports ten missing files. The manifest also does not list this README or the newly written human-readable summaries. It cannot currently verify the public release as laid out here; it has been preserved unchanged. A manifest reconciled to the actual release paths and contents is needed for that purpose.

[provenance/final-synthesis-run-hashes.json](provenance/final-synthesis-run-hashes.json) is the canonical campaign provenance ledger. It references evidence from the full internal assurance repository, not only this minimal public export. Campaign provenance and public-export file integrity are distinct: the former records the underlying evidence, while a release checksum manifest verifies the bytes of its listed published files against recorded digests. Neither establishes behavioral correctness.

## Publication scope

The initial public release intentionally contains:

- Final campaign synthesis.
- Human-readable result summaries.
- Structured final registers.
- Provenance data.

It intentionally omits:

- Raw cloud logs.
- Machine-specific diagnostics.
- Build/cache artifacts.
- Local environments.
- Noisy internal transcripts.

Additional deep-audit evidence can be published separately if useful to reviewers. References to omitted artifacts preserve campaign provenance rather than imply that every underlying transcript is included here. This publication layer summarizes existing evidence and does not add new engine runs or expand the campaign's conclusions.

## Conclusion

No live Diamond PAU vulnerability was established by this campaign.

The campaign is closed subject to the documented assumptions and unknowns. Universal system verification is not claimed.
