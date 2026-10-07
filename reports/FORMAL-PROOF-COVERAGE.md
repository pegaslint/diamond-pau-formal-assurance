# Formal Proof Coverage

The campaign recorded 21 manifest entries: 19 assurance properties and two support entries. Its results distinguish proofs of specified claims from bounded execution evidence and unresolved modeling. Classifications are not security scores or proof percentages.

Certora is a formal verifier that checks specified rules against a contract model under stated assumptions. Halmos symbolically executes EVM code over recorded bounded domains or fixed paths. Foundry's Forge runs concrete contract tests, including production-path and regression scenarios. These tools supply different kinds of evidence:

1. **FORMAL VERIFICATION:** Certora proved the stated rule or property within the documented assumptions and model. The conclusion applies to that claim and domain.
2. **PARTIAL FORMAL VERIFICATION:** Some subclaims were formally proved, while other meaningful parts remained unresolved or had only bounded production-path evidence.
3. **MULTI-ENGINE ASSURANCE:** Forge/Halmos supplied concrete and bounded symbolic evidence, with targeted mutation discrimination, but no universal theorem is claimed.
4. **BLOCKED / UNRESOLVED:** The chosen formal model was not trustworthy or tractable enough to support a proof of the objective. These are explicit unknowns, not passes or established live bugs.

A successful modeled execution witness confirms that a scenario is possible in the model; it does not prove its substantive assertion. A timeout supplies no completed answer. Neither a successful compilation nor a passing narrow check upgrades an unresolved broader property to a proof.

## Formally verified properties

C03, C09, C10, and C11 are classified VERIFIED-WITH-DOCUMENTED-ASSUMPTIONS. Their verified local or state-transition claims retain the following boundaries.

### C03 — RateLimits authorization

Certora verified four rules establishing the scoped authorization boundary: current RateLimits CONTROLLER members may consume or replenish budgets, its own admin role gates configuration, and revocation takes effect. Authority in the shared or proxy role stores does not itself grant RateLimits authority.

The proof uses actual scoped role behavior and the documented production-valid starting state, including a fixed CONTROLLER admin-role relationship in a positive rule. It does not prove that arbitrary role-admin arrangements or future governance configurations satisfy those assumptions. Halmos supplied one passing authorization/revocation check, and both engines detected a targeted authorization alteration. Forge supplied local preflight only; no independent property-test count is inferred. Whole-system authority and complete finite accounting are outside this claim.

### C09 — Finite consumption and accrual

Certora verified five rules, including an independent integer accrual reference and same-time debit composition. In a valid finite state, readable current capacity follows the capped accrual formula; an authorized debit succeeds when it is within that capacity, stores and returns the remainder, and updates the timestamp. Rejection preserves state, selected distinct-key state is unchanged, and successful debits at the same time cannot jointly overspend the initial capacity.

The domain assumes a valid finite starting state and nondecreasing time. Successful arithmetic retains Solidity's checked multiplication and addition; overflow may revert rather than silently saturate. Halmos supplied three passing core checks, but its separate sequential check timed out and remains unresolved. That timeout neither cancels the scoped Certora proof nor becomes a passing independent check. Forge supplied local preflight only, and Certora/Halmos detected a targeted oversized-debit alteration. This is not an arbitrary-history theorem or a proof of finite credit and unlimited behavior, which have separate properties.

### C10 — Finite replenishment ceilings

Certora verified five rules with successful-scenario and overflow controls. Within the scoped finite domain, a successful authorized credit stores and returns the lesser of current capacity plus credit and maxAmount; the getter also caps successfully returned capacity. The rules cover timestamp updates, selected neighboring-key preservation, invalid configuration rejection, and scoped overflow rejection with rollback.

Stored or returned credit and the capped getter are distinct observations: a capped getter alone would not establish correct stored credit. Checked arithmetic can revert, so the proof does not assert that every credit successfully saturates. Halmos supplied three passing checks using bounded zero-slope credit and fixed full-width witnesses; it did not establish an independent universal full-width, nonzero-slope credit theorem or a separate accrual-addition-overflow witness. Forge supplied local preflight only, and Certora/Halmos detected a targeted cap-removal alteration. The represented domains do not establish a universal overflow/history theorem.

### C11 — Explicit unlimited mode and key isolation

Certora verified five rules with constructed unlimited/disabled scenario controls. The unlimited sentinel is a per-key configuration: scoped authorized triggers retain unlimited behavior without accounting writes to the selected key or the checked distinct key. Unlimited mode does not remove role requirements. Disabled-trigger rejection and explicit authorized mode changes were examined within the recorded states.

Halmos supplied four passing checks, including full-width amounts in two checks, and Certora/Halmos detected a targeted neighboring-key alteration. Forge supplied local preflight only. Full-width amount coverage does not mean arbitrary-state coverage: setter isolation and configured-disabled evidence remain limited to constructed states and selected key frames, meaning checks of which key fields remain unchanged. Key distinctness and the declared exclusion of hash collisions are assumptions, not a cryptographic proof. No universal successful-setter neighbor theorem, arbitrary configured-disabled full-field theorem, or exhaustive storage/event-history theorem is claimed.

## Partially verified properties

### C04 — AccessControls assumptions and role-admin structure

Certora verified three local authorization/admin-graph rules, but three positive obligations concerning role-member enumeration and removal remained unresolved. These obligations seek successful modeled examples of the relevant structure, not merely rejection of unauthorized actions. Reasoning that the invariant holds from arbitrary enumerable starting states also remained unresolved or modeling-limited.

Halmos supplied four passing bounded traces and Forge local preflight; these traces do not replace the missing arbitrary-state reasoning. Certora/Halmos detected a targeted unauthorized-grant alteration, which does not resolve the structural gaps. C04 therefore remains PARTIALLY-VERIFIED.

### R04 — Local minimum arithmetic versus the production path

Certora genuinely proved the zero-case and represented-threshold minimum subcore: the specified minimum-decision rules hold within their stated arithmetic domains. It also detected a targeted non-historical threshold alteration. Successful-scenario controls remain distinct from the substantive assertions. The documented slippage range restricts the theorem domain; it is not an extra production guard.

The composed production path received separate boundary assurance: Forge recorded 14 passing tests across the layers, and Halmos recorded 13 passing checks across 32 paths. The symbolic amount-delta equivalence check timed out and remains unresolved. Full expected-amount, liquidity, and tick-to-price arithmetic equivalence was not established, nor was a full-width nonzero threshold theorem.

R04 is the clearest example of a genuine theorem for one layer without a proof of the full production path. Neither the local theorem nor the bounded composition evidence proves the other layer. The complete property remains PARTIALLY-VERIFIED.

## Multi-engine assurance

C01, G03, G04, G05, R01, R02, R03, and R05 are classified MULTI-ENGINE-ASSURANCE. Forge exercised concrete production-faithful paths, while Halmos explored the recorded bounded symbolic EVM domains or fixed paths. Targeted mutation discrimination showed that each setup could detect at least one selected semantic defect in deliberately altered code.

These bounded scenes are not universal theorem proofs. One detected mutant does not establish mutation completeness, and deterministic external endpoints do not establish live protocol economics. The evidence does not cover all possible starting states, histories, call data, callbacks, or surrounding storage.

C01 covered selected privileged roles and entry points, including Controller routing. G03 covered bounded UniV3 principal-budget accounting and atomic failures, excluding accrued fees. G04 covered distinct issuance/repayment keys with a deterministic vault and bounded amounts. G05 covered directional swap budgets in USDC units with the represented conversion factor, without an arbitrary-factor or universal refill-progress theorem. Certora compiled original and altered code for these four properties only; no Certora proof or cloud mutation discrimination is claimed.

R01 depended on production ordering and fee-bearing removal behavior: collecting old fees before the balance baseline keeps them separate from principal. A copied local predicate would not establish that composed behavior. Forge/Halmos supplied bounded paired evidence under the assumption of no new fees between collections; Certora was intentionally not pursued.

R02 supplied bounded direct-claim capability evidence, but Certora feasibility remains BLOCKED-MODELING / UNRESOLVED. Four obligations were verified and two failed; enabled-claim correspondence and disabled-claim-versus-withdrawal remained unresolved. Successful modeled witnesses did not close those assertions, and an undefined return from a reverted modeled call is not evidence of a reachable production reward transfer. No Certora mutation discrimination was claimed. The overall result is bounded direct-claim assurance, not formal verification of the capability objective.

R03 is an alternate-path/trace property: enumerated withdrawal sequences were checked for reward-claim bypass, while direct claims retained their separate gate. Certora was intentionally not pursued. The evidence does not prove universal callback or path completeness. A stale IFarmFacet notice remains a documentation defect, not an established runtime vulnerability.

R05 supplied bounded partition/chunk accounting evidence for fee allowances and logical-transfer-level global/domain consumption. Its separate arithmetic-reference check timed out and remains unresolved despite the passing composed checks. Certora was intentionally not pursued. There is no proof for arbitrarily many chunks, universal transfer progress, actual charged fees, or bridge delivery/economics.

## Modeling-blocked / unresolved properties

C02, C05, C06, C08, and G02 remain explicit unknowns. Their primary formal-model boundaries are:

- **C02:** Successful low-level call/delegatecall paths could not be established reliably in the model. Two Halmos checks passed with an explained warning, but no Forge property-test matrix or accepted mutation closure was asserted. Its committed primary classification is UNKNOWN; BLOCKED-MODELING describes its Certora boundary.
- **C05:** Fallback/delegatecall tracing and memory modeling remained blocked, including unaligned-memory handling. Two fixed Forge tests and two fixed Halmos checks passed, while Certora recorded substantive assertion failures alongside these modeling limits. Fixed checks do not prove exact forwarding for arbitrary calls.
- **C06:** Nested dynamic Config[] / Wire[] returns prevented a successful configuration-read witness. Four fixed Forge tests and four fixed Halmos checks do not establish the general Beacon/Controller capability boundary. Its classification remains UNRESOLVED / BLOCKED-MODELING.
- **C08:** Establishing successful scene/setup behavior was unstable for unexplained reasons. Two fixed Forge tests and two fixed Halmos checks passed, but Certora assertion failures did not close the required-key objective.
- **G02:** Setup, loops, hashing, and call tracing limited the model, which did not establish a successful scenario. Three fixed Forge tests and three fixed Halmos checks do not prove the broader excluded-deposit-direction objective.

These outcomes do not mean that the property passed, that it failed in production, or that a vulnerability was found. They mean that the chosen formal model could not establish a trustworthy theorem for the objective. Narrow passing checks and failed modeled obligations retain their respective limits.

## Why we used multiple engines

Certora provided theorem-style proofs where faithful modeling was tractable. Halmos supplied symbolic EVM execution over bounded domains, and Foundry/Forge supplied concrete production-path execution and regression scenarios. These approaches examine different parts of the behavior and can expose different gaps.

Mutation discrimination checks whether the assurance setup detects targeted semantic defects, with usable original and altered-code controls. It strengthens the interpretation of the selected checks without establishing coverage of every possible defect. Deployment/runtime reconciliation checks that exercised code belongs to the intended executable/source family under the frozen profile and declared immutable differences; code binding alone does not prove behavior or external economic validity.

The evidence is complementary. Combining engines does not mathematically produce universal verification, extend a theorem beyond its assumptions, or resolve a timeout or blocked model.

## Summary

| Category | Properties | Meaning |
|---|---|---|
| VERIFIED-WITH-DOCUMENTED-ASSUMPTIONS | C03, C09, C10, C11 | Four properties with genuine scoped Certora local/state-transition verification under documented assumptions. |
| PARTIALLY-VERIFIED | C04, R04 | Two properties with proved subclaims and meaningful unresolved or bounded-only parts. |
| MULTI-ENGINE-ASSURANCE | C01, G03, G04, G05, R01, R02, R03, R05 | Eight properties with complementary concrete/bounded symbolic evidence and targeted defect discrimination; no universal theorem. |
| UNKNOWN / BLOCKED-MODELING | C02, C05, C08, G02 | Four unknown objectives with formal modeling blockers; C02's committed primary label is UNKNOWN. |
| UNRESOLVED / BLOCKED-MODELING | C06 | One unresolved objective whose nested-return modeling blocked a successful witness. |

C07 = REUSE: existing Beacon specifications are available, but no accepted independent campaign execution or proof is claimed. G01 = FIXTURE: frozen configuration/activity reconciliation supports interpretation, but does not prove future governance behavior. Neither is an independent assurance result.

These five result groups account for 19 assurance properties; the two support entries complete the 21-entry manifest. Partial and multi-engine results can retain additional unresolved engine obligations, as R04 and R02 demonstrate.

## Interpretation

The campaign produced genuine formal proofs for selected local/state-transition claims and bounded multi-engine assurance for several composed production paths. It did not formally verify the Diamond PAU system as a whole. Modeling-blocked properties remain explicit unknowns.

No live Diamond PAU vulnerability was established by this campaign. This finding does not imply that every possible vulnerability has been excluded.
