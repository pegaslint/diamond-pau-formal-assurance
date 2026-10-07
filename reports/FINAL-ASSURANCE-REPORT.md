# Revision 2B final campaign synthesis / closeout

Starting checkpoint: `cd81936f42a5af47068a6bbc85cd18831ff9a066`. This is reconciliation of committed evidence only. No engine, compiler, RPC, new property or mutation execution occurred. Revision 2A, all prior closures/syntheses, Wave-4 planning and canonical source remain unchanged.

No live Diamond PAU vulnerability was established by this campaign. No tested historical regression was shown live in current scoped behavior. The campaign is CLOSED subject to its documented assumptions, explicit unknowns and nonclaims. This is not a universal proof of the Diamond PAU system.

## Complete property accounting

The manifest contains 21 entries: 19 property results, C07 REUSE and G01 FIXTURE. Four property results are VERIFIED-WITH-DOCUMENTED-ASSUMPTIONS, two PARTIALLY-VERIFIED, eight MULTI-ENGINE-ASSURANCE, four UNKNOWN with BLOCKED-MODELING engine qualifications and one UNRESOLVED / BLOCKED-MODELING. C02's committed primary classification is UNKNOWN; its Certora boundary remains BLOCKED-MODELING. REUSE/FIXTURE are not counted as independent proofs.

MULTI-ENGINE-ASSURANCE means complementary concrete production-path execution and bounded symbolic/fixed EVM path evidence, with documented scope and targeted defect discrimination; it is not a universal theorem. Counts measure recorded results and are not a proof percentage.

The exact 21 objectives, priorities, source lines, domains, authoritative engine records and full limitations are in [property-register.json](../data/property-register.json) and [property-register.md](../reports/PROPERTY-REGISTER.md). This matrix retains exact manifest objectives; compact engine descriptions never supersede the cited closures.

| Property | Exact objective | Priority / wave | Forge | Halmos | Certora | Classification | Main limitation | Live established? |
|---|---|---|---|---|---|---|---|---|
| R01 | Old accrued fees must not distort principal slippage or make an otherwise admissible removal unreachable. | P0 / W4 | 15 tests PASS | 6 checks / 23 paths PASS | Intentionally not pursued | MULTI-ENGINE-ASSURANCE | Bounded old fees; no new fees between paired collects; selected frames | No |
| R02 | A reward claim requires its own configured claim capability. | P0 / W4 | 12 tests PASS | 6 checks / 14 paths PASS | BLOCKED-MODELING / UNRESOLVED; 4 verified / 2 failed obligations | MULTI-ENGINE-ASSURANCE | Direct claim only; Certora obligations modeling-unresolved; R03 excluded | No |
| R03 | No Farm withdrawal path may bypass the distinct claim permission. | P0 / W4 | 13 tests PASS | 5 checks / 13 paths PASS | Intentionally not pursued | MULTI-ENGINE-ASSURANCE | Enumerated withdrawal traces only; adversarial callback completeness unproved; stale notice | No |
| R04 | Expected amounts and minimum bounds agree with an independent integer reference at tick boundaries. | P0 / W4 | 14 tests PASS, layers separated | 13 checks / 32 paths PASS; delta timeout 97 paths | Zero and represented-threshold scoped verification; mutant discriminated | PARTIALLY-VERIFIED | Scoped zero/represented-threshold proofs; amount-delta equivalence timeout; bounded composition | No |
| R05 | Splitting a transfer cannot inflate its fee cap or erase global/domain consumption. | P0 / W4 | 20 tests PASS | 4 checks / 60 paths PASS; reference timeout 15 paths | Intentionally not pursued | MULTI-ENGINE-ASSURANCE | Bounded chunks; arithmetic reference timeout; actual fees/bridge economics unproved | No |
| C01 | Each privileged facet entry point enforces its declared role, including through Controller dispatch. | P0 / W3 | 6 tests PASS | 6 checks / 6 paths PASS | Original/mutant compilation only; no cloud proof | MULTI-ENGINE-ASSURANCE | Selected role/entrypoint matrix, not arbitrary calldata/history | No |
| C02 | Custody execution is available only to a caller with ALMProxy CONTROLLER membership. | P0 / W1 | Not asserted as property-test matrix | 2 checks PASS-WITH-EXPLAINED-WARN | Positive call canaries BLOCKED-MODELING | UNKNOWN / BLOCKED-MODELING | Positive low-level call/delegatecall path blocked; no accepted mutation | No |
| C03 | Only current RateLimits CONTROLLER members may consume or replenish; configuration requires its own admin role. | P0 / W1 | Local preflight; no inferred test count | 1 authorization/revocation check PASS | 4 original rules PASS | VERIFIED-WITH-DOCUMENTED-ASSUMPTIONS | Documented fixed CONTROLLER admin pointer / production-valid prestate | No |
| C04 | Authorization assumptions reflect real AccessControls behavior and the frozen role-admin graph. | P0 / W1 | Local preflight; no inferred test count | 4 bounded traces PASS | 3 rules PASS; 3 positive obligations UNRESOLVED; invariant limits retained | PARTIALLY-VERIFIED | Positive enumerable/removal witnesses and arbitrary-state induction unresolved | No |
| C05 | Every fallback call reaches the dispatch for its incoming selector and forwards its exact delegate selector, payload and result. | P0 / W2 | 2 fixed tests PASS | 2 fixed checks PASS | 7 substantive failures; SAT witnesses; memory/calltrace blocker | UNKNOWN / BLOCKED-MODELING | Fallback calltrace / unaligned memory; fixed local evidence only | No |
| C06 | Beacon-only changes cannot silently enlarge Controller capability; cache changes require authorized explicit Controller update/remove. | P0 / W2 | 4 fixed tests PASS | 4 fixed checks PASS | Positive getConfigs witness UNSAT; BLOCKED-MODELING | UNRESOLVED / BLOCKED-MODELING | Nested Config[]/Wire[] return modeling blocks positive witness | No |
| C07 | Untrusted actors cannot install or remove global routing. | P0 / REUSE | Existing specialized Beacon CVL specs available for REUSE; no independently accepted Revision 2B execution closure | No independent result | No independent proof | REUSE | Existing spec availability is not executed proof; inherited structural/loop/empty-set modeling limits require review | No |
| C08 | A disabled required key prevents capability regardless of unrelated configured keys. | P0 / W2 | 2 fixed tests PASS | 2 fixed checks PASS | 9 assertion failures; SAT witnesses; setup cause unresolved | UNKNOWN / BLOCKED-MODELING | Setup-success instability remains unexplained | No |
| C09 | Finite consumption cannot exceed current replenished capacity and cannot double-spend it. | P0 / W1 | Local preflight; no inferred test count | 3 core checks PASS; sequential timeout | 5 original rules PASS; independent accrual reference | VERIFIED-WITH-DOCUMENTED-ASSUMPTIONS | Valid finite state/monotone time; sequential Halmos timeout unresolved | No |
| C10 | Credit and slope accrual never produce successful finite capacity above maxAmount. | P1 / W1 | Local preflight; no inferred test count | 3 checks PASS | 5 rules PASS plus satisfy controls | VERIFIED-WITH-DOCUMENTED-ASSUMPTIONS | Finite represented credit domains; getter cap distinct from stored/returned credit | No |
| C11 | Unlimited is a per-key explicit configuration, not a way to disable roles or modify finite neighbors. | P1 / W1 | Local preflight; no inferred test count | 4 checks PASS | 5 rules PASS plus satisfy controls | VERIFIED-WITH-DOCUMENTED-ASSUMPTIONS | Constructed unlimited/disabled states, selected key frames | No |
| G01 | The frozen capability manifest, key derivations and replayed configuration/activity sets agree exactly. | P1 / FIXTURE | Offline frozen event/configuration fixture validation; no independent solver proof | No independent result | No independent proof | FIXTURE | Finite frozen history completeness; configured differs from enabled; future governance state excluded | No |
| G02 | The two specifically excluded Basin/USDC deposit directions stay unavailable until an explicit authorized configuration. | P0 / W2 | 3 fixed tests PASS | 3 fixed checks PASS | Successful-scene satisfy UNSAT; loop/hash/calltrace limits | UNKNOWN / BLOCKED-MODELING | Setup, loop/hash and calltrace limits; successful scene not proved | No |
| G03 | Each Grove UniV3 operation uses its intended pool/token budgets with exact measured amounts and atomic failures. | P0 / W3 | 10 tests PASS | 10 checks / 17 paths PASS | Original/mutant compilation only; no cloud proof | MULTI-ENGINE-ASSURANCE | Bounded UniV3 principal budgets; accrued fees excluded | No |
| G04 | USDS issuance and repayment use distinct keys, with optional mint replenishment only on burn. | P1 / W3 | 6 tests PASS | 6 checks / 18 paths PASS | Original/mutant compilation only; no cloud proof | MULTI-ENGINE-ASSURANCE | Fixed scenes, bounded amounts, deterministic vault; no live economics | No |
| G05 | Each swap consumes its own directional key in USDC units; only USDC-to-USDS optionally replenishes the opposite key. | P0 / W3 | 9 tests PASS | 9 checks / 31 paths PASS | Original/mutant compilation only; no cloud proof | MULTI-ENGINE-ASSURANCE | Bounded USDC units and factor1e12; no arbitrary-factor theorem | No |

## Formal coverage and engine boundaries

C03, C09, C10 and C11 have genuine Certora property/state-transition verification within documented assumptions. C03 covers the controller authorization boundary with its fixed production-valid admin-pointer prestate. C09 includes an independent integer accrual reference and same-time debit composition; its separate sequential Halmos timeout is unresolved. C10 verifies represented finite replenishment cap behavior and distinguishes stored/returned credit from the capped getter. C11 verifies represented unlimited/disabled and selected key-isolation behavior.

C04 remains partial: three local authorization/admin-graph rules passed, while three positive enumerable/removal witnesses and arbitrary enumerable-state invariant obligations remained unresolved or modeling-limited. Bounded member traces do not establish an arbitrary-state induction theorem.

R04 remains PARTIALLY-VERIFIED. Local zero and represented-threshold obligations have genuine Certora verification; SAT satisfy/generated non-vacuity controls are distinct from substantive assertions. Symbolic amount-delta equivalence timed out (97 explored paths, 60.75 seconds) and is unresolved. Layer B is bounded production-path assurance. Neither layer establishes full universal TickMath/Q96/liquidity arithmetic equivalence or proves the other layer. Its documented slippage range is a theorem-domain restriction, not an invented production guard.

R02's Certora feasibility is BLOCKED-MODELING / UNRESOLVED: shell exit 1, internal exit 100, no timeout, SAT positive and generated non-vacuity witnesses, verified scene binding/authorized actor/zero claim maximum, four verified and two failed obligations. Disabled-claim-versus-withdrawal and enabled-claim correspondence remain unresolved. No CALLTRACE, unaligned-memory, AUTO-havoc or sighash-resolution diagnostic established the cause. Undefined return on a reverted modeled counterexample is not evidence of a reachable production reward transfer. The original model was not repaired or used for cloud mutation discrimination.

Wave 2 retains its architecture-heavy unknowns: C06 nested Config[]/Wire[] returns, C05 fallback/delegatecall memory/calltrace, C08 setup-success instability, and G02 setup/loop/hash/calltrace. C02 retains low-level call/delegate positive-path modeling difficulty. Narrow passing checks and fixed witnesses do not close these properties.

Wave 3 used concrete production scenes and bounded Halmos paths deliberately; Certora original/mutant compilation is never called proof. R01/R03/R05 intentionally did not pursue low-value copied local predicates or broad composition models. R05's standalone reference inequality timed out (15 paths, 60.25 seconds) and remains unresolved even though four composed checks/60 paths passed. No arbitrary-N induction or timeout-as-PASS inference is made.

Forge covers concrete transitions, rejection, rollback, fee/capability/partition matrices and targeted mutations. Halmos covers the recorded bounded domains/fixed paths and targeted mutants. Certora supplies only its actual verified local/property claims. Their evidence is complementary and is not mathematically combined into a universal proof.

## Historical regressions and mutation discrimination

| Regression | Property | Mutation fidelity | Discrimination | Residual boundary |
|---|---|---|---|---|
| REG-001 / M-01 | R01 | EXACT REPAIR-HUNK REVERSAL in current family | Forge + Halmos | Bounded old-fee domains; no new fees between paired collects |
| REG-002 / L-10 | R02 | HISTORICAL BEHAVIOR RESTORATION: ungated direct claim | Forge + Halmos | Direct path only; unresolved Certora feasibility |
| REG-003 / L-13 | R03 | HISTORICAL BEHAVIOR RESTORATION: withdrawal reward helper | Forge + Halmos | Enumerated traces; adversarial callback completeness unproved |
| REG-004 / L-07 | R04 | TARGETED NON-HISTORICAL off-by-one threshold mutation | Forge + Halmos + scoped Certora | Not an exact reconstruction of REG-004 vulnerable source |
| REG-005 / L-11 | R05 | HISTORICAL-STYLE full-transfer cap reuse per chunk | Forge + Halmos | Historical API/tail guard not restored; no claim of exact old whole-API trace |

Fourteen executed targeted mutants were discriminated: five Wave-1 mutants by Certora/Halmos, four Wave-3 mutants by Forge/Halmos, and five Wave-4 mutants by Forge/Halmos, with R04 additionally killed by scoped Certora. This yields 14 recorded Halmos kills, six Certora kills and nine explicitly claimed Forge kills. C02 and the four Wave-2 mutations were unexecuted because the original model was unstable; C07/G01 have no accepted independent mutation closure. Usable positive controls and exact intended assertions remain in the mutation register. One killed mutant per executed property is not mutation or path completeness.

R01 separates principal, old fees and removal admissibility; old fees cannot be silently included in the principal reference. G03 principal-budget accounting did not already prove R01. R02 direct-claim capability is distinct from R03 alternate withdrawal traces. R03 current withdrawal performs legitimate principal withdrawal without getReward, even with claim enabled; this does not prove every conceivable callback trace. R05 splitting may decrease allowed fees due to flooring: for amount23/rate3333, [23] permits7, [12,11] permits6 and [10,10,3] permits6. Global/domain debits remain logical-transfer-level in scope; fee allowances are permitted API arguments, not measured charged fees.

## Frozen deployment / source / runtime binding

The frozen mainnet baseline is block26101248, hash `0x651cdd1721e1dc15affdf46d1a8d386c7a4d21db3a721f3fe9487465bf55006a`, timestamp1790903255 (2026-10-02 01:07:35 UTC), provider label Tenderly. No RPC occurred during closeout.

Representative v1.13.0 source/runtime binding is retained for Controller, AccessControls, ALMProxy and RateLimits. Controller executable comparison uses declared Beacon immutable spans (641/32 and 2351/32) plus metadata removal; the other three core matches require metadata removal without immutable adjustment. Representative commit `5c5ad6ae174bf467081ca82342ced2bd42a5c732` is a compiler-family binding, not unique historical-tag attribution. Canonical pin remains `71604ddcee14cff3561fff66eac17e749c66384b`.

Beacon has 25 integrations; Grove caches four (USDS, PSM, Basin, UniV3), with 47/47 selector wires matching. The other21 are not automatically cached. A Beacon223 selector total is not asserted without a cited current-context count. The finite namespace has190 configuration/100 activity events,18 configured/18 explained keys,zero missing/extra,11 active keys and zero active-never-configured. Configured history is distinct from presently enabled capability. Shared AccessControls, ALMProxy and RateLimits role stores remain distinct authority domains; G01 is frozen fixture support, not a governance induction proof. Source/executable binding alone does not prove behavior.

## Runtime warning reconciliation

| Wave | Occurrences | METADATA-ONLY | IMMUTABLE-ADJUSTED | EXECUTABLE-EXACT | Unexplained |
|---|---:|---:|---:|---:|---:|
| 1 | 33 | 33 | 0 | 0 | 0 |
| 2 | 20 | 0 | 0 | 20 | 0 |
| 3 | 204 | 142 | 62 | 0 | 0 |
| 4 | 60 | 40 | 20 | 0 | 0 |
| Total | 317 | 215 | 82 | 20 | 0 |

Counts are comparable accepted-ledger warning occurrences, not displayed fragments or artifact candidates. Wave2 full-runtime exact candidates take EXECUTABLE-EXACT exclusively even when other candidate artifacts differ only in metadata. Wave1 exact executable equality after metadata removal is METADATA-ONLY, not an extra count. R04 retains60 warnings:40 metadata-only/20 declared immutable-adjusted/zero unexplained. All317 accepted-ledger warnings are accounted for; zero executable mismatches remain unexplained. No recompilation or fresh engine warning generation occurred.

## Documentation defect

R03's IFarmFacet contract-level notice that withdrawal also claims rewards is STALE-DOCUMENTATION, not an established runtime defect. It was introduced April9,2026 and survived the May11 repair unchanged. The repair changed function-level documentation, removed withdrawal reward return/helper behavior and changed the regression test to expect zero rewards. Current implementation, regression intent and R03 manifest agree. Exact hashes/timeline are carried forward from semantic-conflict analysis through the documentation-defect register. Canonical source was not modified.

## Assumptions, nonclaims and unresolved register

Deployment claims depend on the frozen source/runtime profile and declared immutables. Authorization evidence uses actual scoped roles and valid construction; it does not prove arbitrary future governance configurations. Arithmetic claims retain widths, integer-floor order, valid-prestate/time assumptions and explicit domains. Composed paths use deterministic stateful protocol endpoints: these preserve observed production calls but do not establish live economic/protocol validity.

Property-specific distinctions remain intact: C04 bounded members do not close arbitrary enumerable prestates; C09 sequential timeout remains unknown; G04/G05 external accounting scenes do not prove economics or arbitrary factors; G03 excludes accrued-fee behavior. R01 bounded fee0/fee1 and no newly accrued fees between paired collects remain assumptions. R02 covers bounded direct capability combinations, not alternate paths. R03 covers enumerated paths and selected frames, not universal adversarial callbacks. R04 scopes its local proof and retains unresolved amount-delta equivalence. R05 scopes amount/chunk/rate domains, retains the arithmetic timeout and does not prove bridge delivery or actual charged fees. Complete preserved assumption registers are in `final/assumptions.json`.

There is no absence-of-all-vulnerabilities claim, arbitrary-history/calldata theorem, exhaustive storage-frame proof, universal callback theorem, arbitrary-N chunk theorem or mutation-completeness claim. Deterministic endpoints, compilation success and executable binding are not correctness proofs. Blocked modeling and timeouts remain explicit unknowns. No failed modeled obligation is upgraded to a live production counterexample without reachable evidence.

## Methodology progression and closeout

Wave1 established useful local/state-transition formal claims while preserving partial/unknown boundaries. Wave2 established recurring architecture-heavy modeling limits and applied stop-loss. Wave3 deliberately used bounded multi-engine production-path assurance. Wave4 kept local formal arithmetic separate from composition, obtained scoped R04 proof, stopped unresolved R02 feasibility and retained R05 timeout. This progression improves scope honesty without treating bounded assurance as universal verification or lack of universal proof as a campaign failure. No NONDET replacement of core behavior, optimistic loop/hash proof forcing or ghost replacement is introduced by synthesis.

Credentials remain behind the human cloud-execution boundary. Scripts check credential existence without printing it, unset it after runs, keep token-bearing raw logs ignored/local-only and use redacted derivatives with separate raw SHA256 provenance. This closeout neither accesses credentials nor reads/rehashes ignored raw cloud logs. Staged live-token/secret scans and preservation/whitespace gates must pass before handoff.

The campaign can close with all21 entries accounted for,14 targeted mutants discriminated where executed,zero unexplained executable mismatches and all limitations retained. Closure records work completed, not resolution of every unknown. Reopening is justified by a reachable production counterexample, source/configuration/binding change, a concrete faithful tool/model fix, or an explicitly authorized new coverage requirement. No further property wave is recommended automatically.

## Provenance and handoff

`final/checkpoints.json` records checkpoint ancestry, baseline source pin, prior tool-version records, input hashes and starting cleanliness. `final/final-synthesis-run-hashes.json` hashes committed manifest, wave syntheses, closures, run-hash ledgers, mutation diffs, binding/runtime evidence and new synthesis/report bytes; it excludes its own digest and does not turn ignored raw logs into new claims. Existing tool transcripts are preserved byte-for-byte. Only final synthesis artifacts and an append-only status entry are staged. No commit is made.
