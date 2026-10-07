# Property Results

The campaign used a 21-entry manifest: 19 assurance properties, C07 as REUSE, and G01 as FIXTURE. The classifications describe recorded evidence and its limits; they are not percentages or security scores. MULTI-ENGINE-ASSURANCE means bounded evidence from complementary engines, not universal formal proof.

The final classification summary is:

- 4 VERIFIED-WITH-DOCUMENTED-ASSUMPTIONS
- 2 PARTIALLY-VERIFIED
- 8 MULTI-ENGINE-ASSURANCE
- 4 UNKNOWN / BLOCKED-MODELING
- 1 UNRESOLVED / BLOCKED-MODELING
- 1 REUSE
- 1 FIXTURE

The four unknowns include C02, whose committed primary classification is UNKNOWN and whose Certora boundary is BLOCKED-MODELING. Its table result preserves that primary classification.

Forge executes concrete tests. Halmos checks symbolic execution within recorded bounds or fixed paths. Certora verifies specified rules within a model and its assumptions; compilation alone is not verification. A successful execution witness shows that a modeled scenario can occur, but does not prove the property's assertion. Here, a key identifies a configured capability or rate-limit budget; a facet is a module reached through Controller routing.

| ID | What we tried to establish | Result | Evidence | Main remaining limitation |
|---|---|---|---|---|
| C01 | Privileged facet functions require their declared roles, including calls routed through Controller. | MULTI-ENGINE-ASSURANCE | Forge: 6 tests; Halmos: 6 checks across 6 paths passed. Certora compiled original and altered code only. | Selected roles and entry points; arbitrary call data and histories are not covered. |
| C02 | Only callers with ALMProxy's CONTROLLER role can execute custody operations. | UNKNOWN | Halmos: 2 checks passed with an explained warning. Certora could not establish successful low-level call paths; no Forge property-test matrix asserted. | Successful call/delegatecall modeling remains blocked; no accepted altered-code check. |
| C03 | Only current RateLimits CONTROLLER members can consume or replenish budgets; configuration requires its separate admin role. | VERIFIED-WITH-DOCUMENTED-ASSUMPTIONS | Certora: 4 rules verified. Halmos: 1 authorization/revocation check passed; Forge local preflight only. | Assumes the documented fixed CONTROLLER admin-role relationship and a production-valid starting state. |
| C04 | Authorization assumptions match actual AccessControls behavior and the frozen role-admin relationships. | PARTIALLY-VERIFIED | Certora: 3 rules verified, 3 successful-scenario obligations unresolved. Halmos: 4 bounded traces passed; Forge local preflight only. | Role-member enumeration/removal witnesses and a proof covering arbitrary starting states remain unresolved. |
| C05 | Fallback routing uses the incoming selector and forwards the exact delegated selector, payload, and result. | UNKNOWN / BLOCKED-MODELING | Forge: 2 fixed tests; Halmos: 2 fixed checks passed. Certora: 7 substantive failures with execution witnesses, alongside memory/call-trace modeling blockers. | Fallback/delegatecall memory and call-trace modeling remain blocked; local checks cover fixed cases only. |
| C06 | Beacon changes alone cannot expand Controller capabilities; cache changes require an authorized explicit Controller update or removal. | UNRESOLVED / BLOCKED-MODELING | Forge: 4 fixed tests; Halmos: 4 fixed checks passed. Certora could not produce a successful configuration-read witness. | Modeling nested configuration and routing arrays blocks the successful-path witness. |
| C07 | Untrusted actors cannot install or remove global routing. | REUSE | Existing specialized Beacon specifications are available for reuse; no accepted independent campaign execution or proof. | Available specifications are not executed proof; structural, loop, and empty-set modeling limits require review. |
| C08 | Disabling a required key prevents the capability even when unrelated keys are configured. | UNKNOWN / BLOCKED-MODELING | Forge: 2 fixed tests; Halmos: 2 fixed checks passed. Certora: 9 assertion failures with execution witnesses; setup cause unresolved. | Unexplained instability in establishing successful setup prevents closure. |
| C09 | Finite budget consumption cannot exceed replenished capacity or spend that capacity twice. | VERIFIED-WITH-DOCUMENTED-ASSUMPTIONS | Certora: 5 rules verified using an independent accrual reference. Halmos: 3 core checks passed, sequential check timed out; Forge local preflight only. | Assumes valid finite state and nondecreasing time; the separate sequential Halmos timeout remains unresolved. |
| C10 | Replenishment and time-based accrual cannot make successfully read finite capacity exceed maxAmount. | VERIFIED-WITH-DOCUMENTED-ASSUMPTIONS | Certora: 5 rules verified with successful-scenario controls. Halmos: 3 checks passed; Forge local preflight only. | Represented finite credit domains only; the capped capacity getter differs from stored or returned credit. |
| C11 | Unlimited capacity is explicitly configured per key and cannot bypass roles or change finite neighboring keys. | VERIFIED-WITH-DOCUMENTED-ASSUMPTIONS | Certora: 5 rules verified with successful-scenario controls. Halmos: 4 checks passed; Forge local preflight only. | Constructed unlimited/disabled states and selected neighboring-key checks; arbitrary surrounding state is not covered. |
| G01 | The frozen capability manifest, derived keys, and replayed configuration/activity records agree. | FIXTURE | Offline validation of frozen event/configuration records; no independent solver proof. | Finite frozen-history coverage; configured does not mean enabled, and future governance states are excluded. |
| G02 | The two specifically excluded Basin/USDC deposit directions remain unavailable until explicitly authorized configuration. | UNKNOWN / BLOCKED-MODELING | Forge: 3 fixed tests; Halmos: 3 fixed checks passed. Certora could not establish a successful modeled scenario. | Setup, loop, hashing, and call-trace limits remain; the successful scenario is unproved. |
| G03 | Grove UniV3 operations use the intended pool/token budgets, debit measured amounts, and undo changes on failure. | MULTI-ENGINE-ASSURANCE | Forge: 10 tests; Halmos: 10 checks across 17 paths passed. Certora compiled original and altered code only. | Bounded principal-budget accounting; accrued fees and live protocol economics are excluded. |
| G04 | USDS issuance and repayment use separate keys; optional issuance-budget replenishment occurs only on burn. | MULTI-ENGINE-ASSURANCE | Forge: 6 tests; Halmos: 6 checks across 18 paths passed. Certora compiled original and altered code only. | Fixed scenarios, bounded amounts, and a deterministic vault model do not establish live economics. |
| G05 | Swaps debit their own directional key in USDC units; only USDC-to-USDS may replenish the opposite key. | MULTI-ENGINE-ASSURANCE | Forge: 9 tests; Halmos: 9 checks across 31 paths passed. Certora compiled original and altered code only. | Bounded amounts and conversion factor 10^12; no proof for arbitrary factors or universal refill progress. |
| R01 | Previously accrued fees do not distort principal slippage checks or prevent an otherwise admissible removal. | MULTI-ENGINE-ASSURANCE | Forge: 15 tests; Halmos: 6 checks across 23 paths passed. Certora was intentionally not pursued. | Bounded old fees, no new fees between paired collections, and selected checks on unaffected state. |
| R02 | A direct reward claim requires its own configured claim capability. | MULTI-ENGINE-ASSURANCE | Forge: 12 tests; Halmos: 6 checks across 14 paths passed. Certora: 4 obligations verified, 2 failed and modeling-unresolved. | Direct claims only; Certora correspondence obligations remain unresolved. Alternate withdrawal paths are a separate property. |
| R03 | Farm withdrawal paths cannot bypass the separate reward-claim permission. | MULTI-ENGINE-ASSURANCE | Forge: 13 tests; Halmos: 5 checks across 13 paths passed. Certora was intentionally not pursued. | Enumerated withdrawal traces only; adversarial callbacks remain unproved. A stale withdrawal notice persists. |
| R04 | Expected token amounts and minimum bounds agree with an independent integer reference at tick boundaries. | PARTIALLY-VERIFIED | Forge: 14 tests passed across separate layers. Halmos: 13 checks across 32 paths passed; amount-delta check timed out. Certora verified scoped zero/threshold rules and detected a targeted alteration. | Zero and represented-threshold proofs only; amount-delta equivalence remains unresolved and composed paths are bounded. |
| R05 | Splitting a transfer cannot increase its permitted fee cap or remove global/domain budget consumption. | MULTI-ENGINE-ASSURANCE | Forge: 20 tests; Halmos: 4 checks across 60 paths passed; arithmetic reference check timed out. Certora was intentionally not pursued. | Bounded chunks; reference timeout unresolved. Arbitrarily many chunks, actual charged fees, and bridge delivery/economics are unproved. |

## Formally verified or partially verified

C03, C09, C10, and C11 have Certora property or state-transition verification within documented assumptions. These cover scoped authorization, finite consumption, finite capacity caps, and explicit unlimited-key behavior respectively. They do not establish arbitrary future governance behavior or arbitrary histories. C09's separate sequential Halmos timeout remains unresolved despite its verified Certora rules.

C04 verifies three local authorization/admin-relationship rules, but successful role-enumeration/removal witnesses and reasoning over arbitrary enumerable starting states remain unresolved. Its bounded membership traces do not supply that missing general proof.

R04 verifies local zero and represented-threshold obligations. Its symbolic amount-delta equivalence check timed out, and its production-path composition evidence is bounded. Neither layer proves universal tick-to-price, liquidity, or expected-amount arithmetic equivalence, nor does either prove the other layer. The documented slippage range restricts the proof domain; it is not an additional production guard.

## Bounded multi-engine assurance

C01, G03, G04, G05, R01, R02, R03, and R05 combine concrete Forge execution with bounded symbolic or fixed-path Halmos checks and targeted altered-code checks that detected the selected defects. These complementary results provide evidence for the recorded scenarios and domains; adding engine results does not create a universal theorem. Deterministic protocol endpoints do not establish live protocol economics, and detecting selected alterations does not establish coverage of all possible defects.

R02 retains unresolved Certora obligations despite its bounded direct-claim evidence. R03 covers enumerated withdrawal traces, not all adversarial callbacks; its stale contract-level notice is a documentation defect, not an established runtime defect. R05 retains an unresolved arithmetic-reference timeout; permitted fee caps are not measurements of actual charged fees.

## Modeling-blocked / unresolved

C02, C05, C06, C08, and G02 remain explicit unknowns rather than passes or failures of production behavior. Their narrow local checks do not close the broader objectives, and failed modeled obligations do not establish reachable production vulnerabilities.

C02 lacks a successful modeled low-level call/delegatecall path. C05 is blocked by fallback memory and delegated-call tracing. C06 cannot establish a successful configuration-read witness because of nested-array return modeling. C08 retains unexplained setup-success instability. G02 retains setup, loop, hashing, and call-trace limits without a proved successful scenario. These limits explain the classifications; they do not demonstrate that the intended behavior fails in deployment.

## Support entries

C07 = REUSE: existing Beacon specifications may support future work, but availability alone is not an independently executed assurance result. Their inherited modeling limits remain subject to review.

G01 = FIXTURE: offline checks reconcile a finite frozen capability/configuration history. This supports interpretation of the property evidence, but does not prove future governance behavior or make every configured key enabled. Neither support entry counts as an independent assurance property or proof.

## Interpretation

No live Diamond PAU vulnerability was established by this campaign. This property matrix does not imply that all Diamond PAU behavior is formally verified or vulnerability-free.
