# Historical Regression Testing

The campaign revisited five previously identified defect classes. Each regression received executable evidence and targeted mutation discrimination: the harnesses checked current behavior and detected a deliberately altered version that violated an intended assertion. Mutation fidelity differed by property; these were not five exact recreations of historical implementations.

No tested historical regression was established as live in the current scoped behavior. This does not prove that no other regression or vulnerability exists. Forge supplies concrete execution evidence, Halmos checks bounded symbolic or fixed EVM paths, and Certora supplies only the specified formal claims within documented models and assumptions. Forge/Halmos evidence is not universal formal proof.

## REG-001 — UniV3 accrued-fee contamination

Property: R01

**Historical defect and practical failure mode.** Previously accrued fees were included in the balance delta used to assess principal removal. This contaminated the principal reference, raising the required minimums and potentially rejecting an otherwise admissible removal on slippage grounds.

**Repaired behavior and challenge.** The repair collects old fees before recording the balance baseline, so the subsequent delta represents principal alone. The harness compared a successful zero-fee baseline with a corresponding fee-bearing removal using admissible principal minimums. The targeted mutation removed the fee pre-collection, reversing the historical two-line repair in the current source family. With principal amounts 2,000/3,000 and old fees 10,000/10,000, the mutant measured 12,000/13,000 and rejected valid principal minimums of 1,980/2,970; the zero-fee control still succeeded.

**Evidence, fidelity, and result.** Forge recorded 15 passing current-behavior tests; Halmos recorded 6 passing checks across 23 paths. Both detected the mutant through the intended fee-bearing admissibility assertion. Mutation fidelity: **EXACT REPAIR-HUNK REVERSAL**, not recreation of the entire old deployment. Certora was intentionally not pursued. R01 remains MULTI-ENGINE-ASSURANCE, with no live regression established in scope.

**Remaining limitation.** Principal, minimums, liquidity, and old fees were fixed or bounded, and no new fees accrued between the paired collections. This does not prove universal fee-growth behavior, removal liveness, or live AMM economics.

## REG-002 — Farm direct claim missing distinct capability

Property: R02

**Historical defect and repaired behavior.** Direct reward claiming lacked its own claim-capability existence gate. The repaired path requires the exact farm claim key to have `maxAmount > 0` before calling `getReward`. This gate establishes that the capability exists; it is not a reward-amount cap or budget debit. Withdrawal capability is distinct: enabling withdrawal does not itself authorize a direct reward claim.

**Challenge and executable evidence.** The harness exercised bounded claim/withdrawal capability combinations, including withdrawal enabled while claim was absent. The mutation removed only the distinct direct-claim gate, restoring the historical ungated behavior while retaining current roles, routing, and token handling. The altered path called `getReward` and transferred 17 reward units where the assertion expected denial; an enabled-claim control succeeded. Forge recorded 12 passing current-behavior tests and Halmos 6 passing checks across 14 paths; both detected the mutant.

**Certora, fidelity, and result.** The original Certora modeling result remains **BLOCKED-MODELING / UNRESOLVED**: four obligations were verified and two failed, leaving disabled-claim-versus-withdrawal and enabled-claim correspondence unresolved. A modeled reverted call with an undefined return does not establish a reachable production reward transfer. No Certora mutation discrimination was claimed. Mutation fidelity: **HISTORICAL BEHAVIOR RESTORATION**, not the whole old source snapshot. R02 remains MULTI-ENGINE-ASSURANCE, with no live regression established in scope.

**Remaining limitation.** Evidence covers bounded direct claims, including capability states and rewards up to 1,000. Alternate withdrawal paths belong to R03; arbitrary callbacks, configurations, and live Farm economics are not proved. The unresolved Certora obligations remain explicit limitations.

## REG-003 — Farm withdrawal reward-helper bypass

Property: R03

**Historical behavior and repaired behavior.** Withdrawal previously invoked an ungated reward helper unconditionally, allowing the withdrawal route to bypass the separate claim capability. Current withdrawal performs principal withdrawal without calling `getReward`, regardless of whether claim capability is enabled. Rewards remain available through the separately gated direct-claim path.

**Challenge, fidelity, and result.** The mutation appended the historical ungated helper behavior to the current withdrawal flow. The harness expected withdrawal to succeed with no reward claims or rewards; the mutant instead transferred 17 reward units alongside 100 principal units, violating that assertion. A usable-mutant control succeeded. Forge recorded 13 passing current-behavior tests and Halmos 5 passing checks across 13 paths; both detected the restoration. Mutation fidelity: **HISTORICAL BEHAVIOR RESTORATION**, not a whole historical snapshot. Certora was intentionally not pursued. R03 remains MULTI-ENGINE-ASSURANCE, with no live regression established in scope.

**Documentation and remaining limitation.** The stale IFarmFacet contract-level notice saying withdrawal also claims rewards is **DOCUMENTATION DEFECT ONLY**. It does not establish a current runtime defect: implementation, regression intent, and the property agree that withdrawal no longer claims rewards. The campaign covered enumerated withdrawal/claim sequences and selected state checks with a deterministic endpoint; universal callback/path completeness, adversarial reentrancy, and arbitrary histories were not proved.

## REG-004 — UniV3 boundary / rounding

Property: R04

**Historical concern and repaired behavior.** The historical defect involved a lower-boundary/rounding discrepancy in expected token amounts. The repaired behavior uses the corrected lower-boundary comparison and round-up amount-delta semantics. Boundary arithmetic matters because integer rounding can change the expected amounts and the minimums at which an operation is accepted; the minimum predicate was tested separately.

**Challenge and evidence.** Forge/Halmos compared boundary amounts and minimum decisions with an independent integer reference, separating local arithmetic from composed production-path execution. Forge recorded 14 passing tests across these layers; Halmos recorded 13 passing checks across 32 paths. Certora verified scoped zero and represented-threshold obligations, meaning only the stated zero cases and threshold domains were proved within their assumptions.

**Mutation fidelity and result.** The mutation changed a nonzero expected-amount minimum comparison from `>= threshold` to `> threshold`, leaving the zero branch unchanged. Equality-success assertions detected the alteration in Forge and Halmos, with above-threshold controls succeeding; Certora also detected it in the scoped represented-threshold rule. Mutation fidelity: **TARGETED NON-HISTORICAL MUTATION**. This was not an exact reconstruction of REG-004, and Certora's discrimination of this threshold mutant does not prove discrimination of the exact historical bad source. R04 remains PARTIALLY-VERIFIED, with no live regression established in scope.

**Remaining limitation.** Symbolic amount-delta equivalence timed out and remains unresolved. The campaign did not prove the full expected-amount/liquidity/TickMath arithmetic chain, universal tick-to-price equivalence, or a full-width nonzero threshold theorem. Bounded composition evidence does not extend the local Certora proofs into a universal theorem.

## REG-005 — CCTP fee reuse / chunk accounting

Property: R05

**Historical defect and practical failure mode.** A fixed fee allowance was reused for each transfer chunk. Repetition could inflate the total permitted fee allowance, while a historical requirement that the fee be smaller than the chunk could reject a small final tail. These are allowance and reachability concerns; allowance arguments do not establish fees actually charged by the bridge.

**Repaired behavior and challenge.** The repair uses a bounded per-domain fee rate and computes each chunk's cap as `floor(chunkAmount * rate / 10000)`. Global and destination-domain budgets each consume the logical transfer amount once, independent of chunk partition. The harness checked per-chunk caps against an independent reference, aggregate allowances, and the scoped global/domain consumption behavior across partitions. For amount 23 and rate 3,333, chunks 10/10/3 receive caps 3/3/0, totaling 6; an unsplit transfer permits 7. Flooring can therefore reduce the allowance when splitting.

**Evidence, fidelity, and result.** Forge recorded 20 passing current-behavior tests; Halmos recorded 4 passing checks across 60 paths. The mutation computed the full-transfer cap of 7 once and reused it for every chunk, producing caps 7/7/7 and total allowance 21 in that example. Both engines detected the intended per-chunk reference mismatch, while a one-chunk mutant control succeeded. Mutation fidelity: **HISTORICAL-STYLE MUTATION**. The current fee-rate API and budgets remained in place, and the historical small-tail guard was not restored; the mutant trace is not claimed to reproduce the complete old API. Certora was intentionally not pursued. R05 remains MULTI-ENGINE-ASSURANCE, with no live regression established in scope.

**Remaining limitation.** A separate arithmetic-reference check timed out and remains unresolved. Amounts, rates, and chunk counts were bounded; arbitrary-N reasoning, meaning a proof for any number of chunks, was not established. Universal transfer progress, arbitrary bridge/token behavior, cross-chain delivery, bridge economics, and actual charged fees remain unproved.

## Summary

| Regression | Property | Historical problem | Mutation fidelity | Engines that discriminated it | Current conclusion |
|---|---|---|---|---|---|
| REG-001 | R01 | Old fees contaminated principal deltas and could falsely reject removal. | EXACT REPAIR-HUNK REVERSAL | Forge, Halmos | MULTI-ENGINE-ASSURANCE; no live regression established in scope. |
| REG-002 | R02 | Direct claims lacked their distinct capability gate. | HISTORICAL BEHAVIOR RESTORATION | Forge, Halmos | MULTI-ENGINE-ASSURANCE; Certora modeling unresolved; no live regression established in scope. |
| REG-003 | R03 | Withdrawal invoked an ungated reward helper. | HISTORICAL BEHAVIOR RESTORATION | Forge, Halmos | MULTI-ENGINE-ASSURANCE; callback/path completeness unproved; no live regression established in scope. |
| REG-004 | R04 | Lower-boundary/rounding discrepancy affected amount calculations. | TARGETED NON-HISTORICAL MUTATION | Forge, Halmos, scoped Certora for the threshold mutant | PARTIALLY-VERIFIED; symbolic amount-delta equivalence unresolved; no live regression established in scope. |
| REG-005 | R05 | Repeated fixed fee caps inflated allowances and could reject a small tail. | HISTORICAL-STYLE MUTATION | Forge, Halmos | MULTI-ENGINE-ASSURANCE; arithmetic reference unresolved; no live regression established in scope. |

## Interpretation

All five historical targets received executable regression evidence, and all five targeted mutants were discriminated by Forge/Halmos. The R04 mutant was additionally discriminated by scoped Certora verification. No live regression was established in the tested scope. Mutation discrimination is not mutation completeness: detecting these selected alterations does not establish detection of every possible defect. Historical behavior fidelity differs by regression, as recorded above.

The regression campaign increases confidence that the documented repairs still separate the tested current behavior from the targeted historical defect classes. It does not establish that all possible regressions or vulnerabilities have been excluded.
