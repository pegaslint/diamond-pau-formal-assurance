# Assumptions and Limits

Every assurance result depends on a defined model and scope. Assumptions are not vulnerabilities: they define when a conclusion applies. Unresolved and modeling-blocked properties remain explicit unknowns. This document explains the campaign's boundaries so readers can assess its results without extending them beyond the evidence.

The limits below apply to the named properties and recorded scenes. An assumption for one property is not automatically an assumption for every other property.

## 1. Deployment and source binding

The campaign checked the intended source family against executable/runtime evidence. Comparisons reconciled metadata-only differences and, where relevant, declared immutable values embedded in compiled code. Controller required the declared Beacon immutable adjustment; AccessControls, ALMProxy, and RateLimits matched after metadata removal without that adjustment. Recorded runtime-warning reconciliation left no unexplained executable mismatch.

This binding establishes that exercised code belongs to the intended executable/source family under the recorded profile. It does not establish behavioral correctness, uniquely identify a historical tag, or demonstrate whole-repository build reproducibility. Pinned dependency snapshots and source provenance support the exercised builds, rather than a broader reproducibility claim.

The deployment evidence uses the frozen baseline at block 26101248, dated 2026-10-02 01:07:35 UTC. Closeout made no new RPC requests. Isolated constructed test scenes are not fresh on-chain deployment attestations or proofs of the live meaning of every immutable address. The frozen routing/configuration evidence does not establish future governance behavior: configured keys need not be enabled, and Beacon integrations are not automatically cached by Controller. G01 is finite fixture support, not a governance induction proof; Farm properties R02/R03 have historical rather than direct Grove relevance.

## 2. Tool and EVM assumptions

The results trust Solidity/compiler/EVM semantics within the recorded toolchain, including checked unsigned arithmetic, delegatecall, and transaction rollback. Forge runs concrete contract tests; Halmos symbolically executes EVM paths over recorded domains; Certora verifies specified rules within its model and assumptions. Results depend on each tool's supported semantics, including scene construction, caller impersonation, funding, time changes, and input constraints used by the harness.

Compilation success establishes build acceptance, not proof or successful execution of the intended scenario. This campaign did not independently prove compiler, solver, or verifier correctness, or prove equivalence across compilers. A successful modeled scenario witness does not by itself prove its assertion or validate every reachable starting-state constraint.

Where storage addressing, derived keys, or snapshots rely on hash collision freedom, that is a declared assumption rather than a cryptographic theorem. Snapshot assumptions apply to the observations that use them, not to every rule. Distinct role stores and keys were retained where required.

## 3. Bounded symbolic domains

Many Halmos checks constrained inputs or used fixed constructed states. A completed symbolic check covers the modeled paths within its recorded domain and supported execution settings; it does not cover every possible uint256 history or starting state. A timed-out check supplies no completed answer. A fixed scene remains fixed even when its recorded output lists no additional loop bounds.

Examples illustrate the differing scopes:

- **G04:** Symbolic issuance amounts were at most 50, with repayment no larger than issuance; budgets, neighboring-key state, and zero slopes were constructed. Full-width arithmetic, arbitrary failures, and unbounded lifecycles were not covered.
- **G05:** Directional amounts ranged from 0 to 20, with separate fixed cases using conversion factor 10^12 and zero, one, or two refills. These checks do not prove arbitrary factors, partitions, or refill progress.
- **R01:** Each symbolic old-fee amount ranged from 0 to 10,000 with fixed principal, minimums, and liquidity; separate fixed large-fee cases were included. No new fees were assumed to accrue between the two collections. The paired scenes were reset without interleaved access to the old scene.
- **R02:** Claim/withdrawal capability combinations and rewards up to 1,000 were checked. This is direct-claim evidence, not coverage of arbitrary rate records, reward tokens, or alternate paths.
- **R03:** Withdrawal/reward inputs were bounded to 1,000 with constructed claim states and enumerated sequences. Callbacks were explicitly excluded from the minimal historical witness.
- **R05:** The composed symbolic domain used transfer amounts at most 30, chunk limit 10, rates at most 9,999, and up to three chunks. A separate arithmetic-reference check remained unresolved.

Other properties have different domains. C01 used six fixed role/entry-point checks with no symbolic input parameters; these covered actors with neither role, admin only, allocator only, or both roles, but not arbitrary calldata or time histories. Its selected UniV3 cases did not include increase-branch controls. G03 used a bounded swap-input range of 4,000–10,000 and fixed principal-accounting scenes, including selected token decimals and price/slippage settings; it did not cover arbitrary decimals, oracle behavior, or accrued fees. Some C11 checks covered full-width amounts while retaining fixed keys and states. Amount breadth must not be mistaken for state breadth.

## 4. Deterministic external endpoints

Several composed properties used deterministic stateful protocol endpoints: test contracts that perform predictable transfers and record calls, balances, counters, or allowances. They preserved the behavior needed for the specific accounting and rollback observations. Their construction/configuration methods are fixture conveniences, not independently assessed live protocol permissions.

For example, G04 used honest 1:1 vault draw/wipe semantics with successful token returns. G05 used funded, fee-free token/migrator/PSM behavior and represented conversion multiplication. G03 used funded principal outputs and valid owned positions, not a full AMM. R01 used reserve-backed old fees and actual transfers while honoring the selected position/minimum/deadline conditions; it excluded fee-on-transfer and rebasing tokens. R02/R03 used funded Farm/token endpoints, and R05 recorded bridge call parameters with actual fixture balance/allowance changes.

These endpoints are not full economic models of live external protocols. Their success does not establish live solvency, bridge finality, AMM economics, or arbitrary adversarial token behavior. Coverage of an explicitly exercised failure is limited to that failure scenario.

## 5. Selected frame and rollback observations

A frame observation checks which parts of state remain unchanged. The campaign observed selected balances, allowances, storage fields, routes, roles, endpoint counters, and budgets, depending on the property. These checks are not always exhaustive storage-frame theorems.

G03 explicitly exercised atomic failures within its composed accounting scope. R03 included a stateful endpoint failure after a withdrawal token transfer to observe rollback. R04 used selected production-path rollback snapshots, and R05 observed selected balances, approvals, and records on rollback. C09/C10 have scoped formal rejection/rollback claims. Each conclusion applies to the recorded transition and observed or proved fields; it should not become a universal no-state-change claim for arbitrary execution.

C11's successful-setter and configured-disabled frames remain limited to constructed states and selected keys. C10 does not establish a universal frame theorem across all setter overloads. R02's minimal token fixture had no approval state and its claim path invoked no approvals, so it supplies no universal approval-preservation theorem. R03 did not exercise the deposit approval flow. C05's observation namespace was kept separate from production storage namespaces, but its controlled recorder does not establish arbitrary malicious-facet isolation.

## 6. Authorization and governance assumptions

Real role stores and authorization paths were retained where relevant. Shared AccessControls, ALMProxy's own roles, and RateLimits' own roles are separate authority domains: a grant in one does not automatically authorize another. Trusted bound facets and linked core contracts underpin the recorded routing scenes; the campaign does not establish isolation from arbitrary malicious delegated code or economic safety for arbitrary authorized targets.

C03 specifically uses a production-valid starting state in which CONTROLLER's admin role is DEFAULT_ADMIN_ROLE, justified by RateLimits' role initialization and absence of a reachable writer for that relationship. This is not a global restriction on AccessControls' admin graph. C04's authorization claim is conditional on an allocator lacking the current admin role; legitimate governance can change role-admin relationships. Frozen memberships initialize the scene rather than establish permanent membership or admin-count invariants.

C09/C10 assume the relevant authorized controller transitions rather than re-prove every authority boundary. C11 retains role requirements for unlimited mode and permits explicit authorized configuration changes. Zero slopes and fixed time in several composed budget scenes do not establish universal time-based replenishment behavior.

No theorem against malicious or compromised governance is implied. Isolated authorized actors and frozen configuration history do not represent every live governance timeline. C07 offers existing Beacon specifications for reuse, without an accepted independent execution/proof result; their structural, loop, and empty-set modeling limits still require review.

## 7. Arithmetic and rounding domains

Integer widths, operation order, flooring/rounding, and representability determine arithmetic claims. Solidity checked overflow/underflow behavior is retained where modeled: an operation may revert rather than successfully saturate at a cap.

C09 assumes valid finite state, with stored capacity within its maximum and stored time no later than current time. Its same-time debit composition is distinct from its unresolved sequential Halmos check. C10's bounded Halmos credit checks used zero slope and fixed overflow witnesses; no independent universal full-width/nonzero-slope credit result or separate accrual-addition-overflow witness is claimed. Successful stored/returned credit is a distinct observation from a capped getter. C11's unlimited path bypasses finite accrual arithmetic, so finite valid-time assumptions are not imposed globally on unlimited states.

R04 has scoped formal proofs for zero expected amounts and represented minimum thresholds. The zero case covers full-width minimum/slippage inputs at zero call value; the represented-threshold domain uses uint128 expected amounts, uint64 slippage no greater than 10^18, and full-width minima at zero call value. This slippage restriction is a theorem domain, not an invented production guard. The independent reference avoids the production FullMath/LiquidityAmounts helpers but shares canonical TickMath ratios. Floored thresholds do not uniquely recover exact expected amounts: different amounts can map to the same threshold.

R04's symbolic amount-delta check used liquidity at most 1,000,000 at selected ticks and timed out; a larger design ceiling is not executed coverage. Its composed boundary cases used fixed ticks, target amounts, and slippage. No full expected-amount/liquidity/TickMath equivalence or full-width nonzero theorem follows.

R05 requires represented chunk-by-rate multiplication for successful arithmetic. A positive chunk limit is needed for progress on positive transfers; a zero limit lies outside that success domain and was not silently assumed to progress. Per-chunk flooring can lower aggregate fee allowances when splitting. The separate arithmetic-reference timeout remains unresolved, without a universal arithmetic or arbitrary-N chunk theorem. R01 likewise retains represented intermediate arithmetic and collectible widths for its selected position/fee domain.

## 8. External protocol and economic limits

The campaign does not establish full AMM solvency, live price/oracle correctness, or general adversarial token behavior. G03's principal-budget result does not already prove R01's old-fee separation. R01 does not establish live fee accrual, fee-only collection liveness, position burning, or complete cleanup, nor that fees cannot affect any conceivable removal.

G04 assumes honest draw/wipe/transfer behavior; arbitrary false-return tokens, protocol failure modes, and live vault/token semantic identity are outside its result. G05 assumes fee-free swaps and funded 1:1 migration for its accounting claims. Finite refill witnesses do not prove universal refill progress or infinite-loop absence; its zero-tranche model reverts. C01's simplified external reachability scenes do not supply fee/refill/economics conclusions.

R02's positive farm claim key establishes capability existence, not a reward-amount cap/debit or reward-token whitelist. R03 does not prove autonomous Farm harvesting, adversarial callbacks/reentrancy, all token/reward identities, or deposit execution/approval behavior. Its stale IFarmFacet withdrawal notice is a documentation defect, not an established runtime vulnerability.

R05's bridge endpoint observes permitted maxFee arguments and fixture token movement, not live burn, charged fees, cross-chain delivery, or finality. Arbitrary bridge/token behavior, arbitrary burn limits, universal progress, and rate refill over time remain outside the proof claims. G02's fixed Basin scenes use funded endpoints and 1:1 shares with valid slippage, without a live economic or role/configuration-history theorem. C08's deterministic token/vault custody model is likewise conditional, and its fixed facet-local evidence is not original Controller-composed execution.

## 9. Historical regression fidelity

The five regression targets received executable evidence and targeted mutation discrimination, meaning the selected checks detected intentionally altered behavior. Fidelity differed:

- **R01 — EXACT REPAIR-HUNK REVERSAL:** The historical repair was reversed within the current source family, without recreating the entire old deployment.
- **R02 — HISTORICAL BEHAVIOR RESTORATION:** The distinct direct-claim gate was removed to restore the historical ungated behavior.
- **R03 — HISTORICAL BEHAVIOR RESTORATION:** The historical ungated withdrawal reward-helper behavior was restored in the current flow.
- **R04 — TARGETED NON-HISTORICAL MUTATION:** A minimum-threshold comparison was altered; this was not an exact reconstruction of the historical boundary/rounding defect.
- **R05 — HISTORICAL-STYLE MUTATION:** A full-transfer fee cap was reused per chunk while retaining the current API; the old small-tail guard was not restored.

All five selected mutants were detected by Forge/Halmos, with R04 additionally detected by scoped Certora verification. Mutation discrimination is not mutation completeness, and a timeout, parser error, or modeling failure is not a detected semantic defect. Planned or unexecuted mutations do not count as coverage. C02 and the four modeling-blocked second-wave properties have no accepted executed mutation closure.

## 10. Modeling-blocked and unresolved properties

The following broad objectives remain open despite narrow fixed or bounded checks:

- **C02 — UNKNOWN:** Successful low-level call/delegatecall paths could not be established in the Certora model; its engine boundary remains BLOCKED-MODELING.
- **C05 — UNKNOWN / BLOCKED-MODELING:** Fallback/delegatecall call tracing and memory modeling remained blocked. The controlled fixed recorder does not prove general payload/result forwarding.
- **C06 — UNRESOLVED / BLOCKED-MODELING:** Nested dynamic Config[] / Wire[] returns blocked a successful configuration-read witness. A simpler dispatch getter cannot substitute for the full configuration objective.
- **C08 — UNKNOWN / BLOCKED-MODELING:** Successful scene/setup behavior remained unstable for unexplained reasons.
- **G02 — UNKNOWN / BLOCKED-MODELING:** Setup, loop, hashing, and call-trace limits prevented closure of the excluded-deposit-direction objective.

Unresolved subclaims also remain within other classifications:

- **C04:** Positive role-enumeration/removal obligations and arbitrary enumerable starting-state reasoning remain unresolved; the Halmos membership scene was bounded to at most two members.
- **C09:** The independent sequential Halmos check timed out, separately from its verified Certora same-time composition rules.
- **R04:** Symbolic amount-delta equivalence timed out; the scoped minimum theorem does not prove the full production path.
- **R02:** Certora remains BLOCKED-MODELING / UNRESOLVED despite four verified obligations; disabled-claim-versus-withdrawal and enabled-claim correspondence failed and remain unresolved. Undefined return data from a reverted modeled call is not a reachable production reward-transfer counterexample.
- **R05:** The standalone arithmetic-reference check timed out; bounded composed partition evidence does not resolve it.

Blocked or unresolved does not mean pass, production failure, or live bug. Proposed internal causes that were not established remain hypotheses, not new assumptions. These results record where the chosen model or completed coverage could not establish the broader claim.

## 11. What this campaign does NOT claim

The campaign does not claim:

- Universal formal verification of Diamond PAU, or a universal theorem obtained by adding engine results.
- Proof of absence of vulnerabilities or of all historical regressions being excluded.
- Arbitrary-history or arbitrary-state coverage, including arbitrary future governance and configurations.
- Exhaustive arbitrary-calldata coverage or an exhaustive storage-frame theorem.
- Universal callback-path coverage, adversarial reentrancy coverage, or R03 path completeness.
- A proof for any number of chunks, arbitrary partitions, or universal transfer/refill liveness.
- Mutation completeness or coverage from planned/unexecuted alterations.
- Live economic correctness of external protocols, arbitrary token behavior, bridge delivery/finality, or actual charged-fee correctness.
- Universal expected-amount, tick-to-price, liquidity, or nonzero minimum arithmetic equivalence beyond R04's stated domains.
- That compilation, successful modeled witnesses, or deterministic endpoint behavior establish substantive correctness.
- That source/runtime binding alone establishes behavioral correctness or safety, uniquely identifies a historical tag, or proves whole-repository reproducibility.
- That timeouts and modeling blockers have been resolved, or that failed modeled obligations establish reachable production exploits.
- That C07 REUSE or G01 FIXTURE supplies an independent assurance proof.

## 12. How to interpret the conclusion

The campaign produced selected local/state-transition formal claims alongside bounded evidence for composed paths. Its classifications retain the differing assumptions and open questions; they do not extend those conclusions to the whole system.

No live Diamond PAU vulnerability was established within the tested scope. That conclusion should be read together with the assumptions, bounded domains, modeling-blocked properties and unresolved claims documented above.
