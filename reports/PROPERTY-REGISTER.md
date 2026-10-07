# Revision 2B final property register

All 21 committed Revision 2A entries are accounted for once. REUSE and FIXTURE are support entries, not independent proofs. Exact objectives below come from the manifest; final outcomes retain closure qualifiers.

## R01 — MULTI-ENGINE-ASSURANCE

> Old accrued fees must not distort principal slippage or make an otherwise admissible removal unreachable.

Priority: P0; assigned wave: W4; entry kind: property result.

Manifest: `evidence/revision2a/01-property-manifest.md:47`. Evidence: `evidence/revision2b/39-R01-final-closure.md`.

Retained limit: Bounded old fees; no new fees between paired collects; selected frames.

## R02 — MULTI-ENGINE-ASSURANCE

> A reward claim requires its own configured claim capability.

Priority: P0; assigned wave: W4; entry kind: property result.

Manifest: `evidence/revision2a/01-property-manifest.md:85`. Evidence: `evidence/revision2b/40-R02-final-closure.md`.

Retained limit: Direct claim only; Certora obligations modeling-unresolved; R03 excluded.

## R03 — MULTI-ENGINE-ASSURANCE

> No Farm withdrawal path may bypass the distinct claim permission.

Priority: P0; assigned wave: W4; entry kind: property result.

Manifest: `evidence/revision2a/01-property-manifest.md:123`. Evidence: `evidence/revision2b/41-R03-final-closure.md`.

Retained limit: Enumerated withdrawal traces only; adversarial callback completeness unproved; stale notice.

## R04 — PARTIALLY-VERIFIED

> Expected amounts and minimum bounds agree with an independent integer reference at tick boundaries.

Priority: P0; assigned wave: W4; entry kind: property result.

Manifest: `evidence/revision2a/01-property-manifest.md:161`. Evidence: `evidence/revision2b/38-R04-final-closure.md`.

Retained limit: Scoped zero/represented-threshold proofs; amount-delta equivalence timeout; bounded composition.

## R05 — MULTI-ENGINE-ASSURANCE

> Splitting a transfer cannot inflate its fee cap or erase global/domain consumption.

Priority: P0; assigned wave: W4; entry kind: property result.

Manifest: `evidence/revision2a/01-property-manifest.md:199`. Evidence: `evidence/revision2b/42-R05-final-closure.md`.

Retained limit: Bounded chunks; arithmetic reference timeout; actual fees/bridge economics unproved.

## C01 — MULTI-ENGINE-ASSURANCE

> Each privileged facet entry point enforces its declared role, including through Controller dispatch.

Priority: P0; assigned wave: W3; entry kind: property result.

Manifest: `evidence/revision2a/01-property-manifest.md:237`. Evidence: `evidence/revision2b/33-C01-final-closure.md`.

Retained limit: Selected role/entrypoint matrix, not arbitrary calldata/history.

## C02 — UNKNOWN / BLOCKED-MODELING

> Custody execution is available only to a caller with ALMProxy CONTROLLER membership.

Priority: P0; assigned wave: W1; entry kind: property result.

Manifest: `evidence/revision2a/01-property-manifest.md:275`. Evidence: `evidence/revision2b/11-C02-certora-closure-after-canary3.md`.

Retained limit: Positive low-level call/delegatecall path blocked; no accepted mutation.

## C03 — VERIFIED-WITH-DOCUMENTED-ASSUMPTIONS

> Only current RateLimits CONTROLLER members may consume or replenish; configuration requires its own admin role.

Priority: P0; assigned wave: W1; entry kind: property result.

Manifest: `evidence/revision2a/01-property-manifest.md:313`. Evidence: `evidence/revision2b/15-C03-final-closure.md`.

Retained limit: Documented fixed CONTROLLER admin pointer / production-valid prestate.

## C04 — PARTIALLY-VERIFIED

> Authorization assumptions reflect real AccessControls behavior and the frozen role-admin graph.

Priority: P0; assigned wave: W1; entry kind: property result.

Manifest: `evidence/revision2a/01-property-manifest.md:351`. Evidence: `evidence/revision2b/18-C04-final-closure.md`.

Retained limit: Positive enumerable/removal witnesses and arbitrary-state induction unresolved.

## C05 — UNKNOWN / BLOCKED-MODELING

> Every fallback call reaches the dispatch for its incoming selector and forwards its exact delegate selector, payload and result.

Priority: P0; assigned wave: W2; entry kind: property result.

Manifest: `evidence/revision2a/01-property-manifest.md:389`. Evidence: `evidence/revision2b/25-C05-final-closure.md`.

Retained limit: Fallback calltrace / unaligned memory; fixed local evidence only.

## C06 — UNRESOLVED / BLOCKED-MODELING

> Beacon-only changes cannot silently enlarge Controller capability; cache changes require authorized explicit Controller update/remove.

Priority: P0; assigned wave: W2; entry kind: property result.

Manifest: `evidence/revision2a/01-property-manifest.md:427`. Evidence: `evidence/revision2b/23-C06-final-closure.md`.

Retained limit: Nested Config[]/Wire[] return modeling blocks positive witness.

## C07 — REUSE

> Untrusted actors cannot install or remove global routing.

Priority: P0; assigned wave: REUSE; entry kind: reuse primitive.

Manifest: `evidence/revision2a/01-property-manifest.md:465`. Evidence: `evidence/revision2a/01-property-manifest.md:465`.

Retained limit: Existing spec availability is not executed proof; inherited structural/loop/empty-set modeling limits require review.

## C08 — UNKNOWN / BLOCKED-MODELING

> A disabled required key prevents capability regardless of unrelated configured keys.

Priority: P0; assigned wave: W2; entry kind: property result.

Manifest: `evidence/revision2a/01-property-manifest.md:503`. Evidence: `evidence/revision2b/27-C08-final-closure.md`.

Retained limit: Setup-success instability remains unexplained.

## C09 — VERIFIED-WITH-DOCUMENTED-ASSUMPTIONS

> Finite consumption cannot exceed current replenished capacity and cannot double-spend it.

Priority: P0; assigned wave: W1; entry kind: property result.

Manifest: `evidence/revision2a/01-property-manifest.md:541`. Evidence: `evidence/revision2b/19-C09-final-closure.md`.

Retained limit: Valid finite state/monotone time; sequential Halmos timeout unresolved.

## C10 — VERIFIED-WITH-DOCUMENTED-ASSUMPTIONS

> Credit and slope accrual never produce successful finite capacity above maxAmount.

Priority: P1; assigned wave: W1; entry kind: property result.

Manifest: `evidence/revision2a/01-property-manifest.md:579`. Evidence: `evidence/revision2b/20-C10-final-closure.md`.

Retained limit: Finite represented credit domains; getter cap distinct from stored/returned credit.

## C11 — VERIFIED-WITH-DOCUMENTED-ASSUMPTIONS

> Unlimited is a per-key explicit configuration, not a way to disable roles or modify finite neighbors.

Priority: P1; assigned wave: W1; entry kind: property result.

Manifest: `evidence/revision2a/01-property-manifest.md:617`. Evidence: `evidence/revision2b/21-C11-final-closure.md`.

Retained limit: Constructed unlimited/disabled states, selected key frames.

## G01 — FIXTURE

> The frozen capability manifest, key derivations and replayed configuration/activity sets agree exactly.

Priority: P1; assigned wave: FIXTURE; entry kind: fixture/binding support.

Manifest: `evidence/revision2a/01-property-manifest.md:655`. Evidence: `evidence/revision2a/01-property-manifest.md:655`.

Retained limit: Finite frozen history completeness; configured differs from enabled; future governance state excluded.

## G02 — UNKNOWN / BLOCKED-MODELING

> The two specifically excluded Basin/USDC deposit directions stay unavailable until an explicit authorized configuration.

Priority: P0; assigned wave: W2; entry kind: property result.

Manifest: `evidence/revision2a/01-property-manifest.md:693`. Evidence: `evidence/revision2b/29-G02-final-closure.md`.

Retained limit: Setup, loop/hash and calltrace limits; successful scene not proved.

## G03 — MULTI-ENGINE-ASSURANCE

> Each Grove UniV3 operation uses its intended pool/token budgets with exact measured amounts and atomic failures.

Priority: P0; assigned wave: W3; entry kind: property result.

Manifest: `evidence/revision2a/01-property-manifest.md:731`. Evidence: `evidence/revision2b/35-G03-final-closure.md`.

Retained limit: Bounded UniV3 principal budgets; accrued fees excluded.

## G04 — MULTI-ENGINE-ASSURANCE

> USDS issuance and repayment use distinct keys, with optional mint replenishment only on burn.

Priority: P1; assigned wave: W3; entry kind: property result.

Manifest: `evidence/revision2a/01-property-manifest.md:769`. Evidence: `evidence/revision2b/32-G04-final-closure.md`.

Retained limit: Fixed scenes, bounded amounts, deterministic vault; no live economics.

## G05 — MULTI-ENGINE-ASSURANCE

> Each swap consumes its own directional key in USDC units; only USDC-to-USDS optionally replenishes the opposite key.

Priority: P0; assigned wave: W3; entry kind: property result.

Manifest: `evidence/revision2a/01-property-manifest.md:807`. Evidence: `evidence/revision2b/34-G05-final-closure.md`.

Retained limit: Bounded USDC units and factor1e12; no arbitrary-factor theorem.
