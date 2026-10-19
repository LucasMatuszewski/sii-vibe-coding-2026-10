# QA work packages

approval: draft

Date: 2026-10-18. Role: qa-engineer. Registry: [implementation plan](000-implementation-plan.md). This document proposes future implementation work; it reports no runtime tests as passed. The user authorized committing and pushing the current plan; application implementation, paid calls now and deployment are not authorized by that publication instruction.

## Scope and current evidence

The inspected `app/` contains only its scaffold README. No installed test runner, runnable application, actual package scripts, passing test counts, provider capture, or browser result is established by this planning task. All source/test paths below are proposed until P01 records the actual scaffold. Command aliases must be verified against P01's package manifest and lockfile before use. No dependency installation, initialization, runtime test, paid inference, secret provisioning, or production edit belongs to this document task.

Normative sources are [AGENTS.md](../../AGENTS.md), [PRD](../PRD.md), [ADR-002](../ADR/002-backend-and-ai.md), [ADR-003](../ADR/003-frontend-and-chat-ui.md), [ADR-004](../ADR/004-verification-and-boundaries.md), [design guidelines](../design-guidelines.md), [tokens](../../assets/design-tokens.json), and [brand screenshot](../../assets/homepage.png). Read only the sections named by a package. Teaching examples under `course-materials/` are not inputs. PRD §11 defines image observation, permissions, prose and disclaimer; §12 defines Polish submission-date assumptions and unresolved company/legal inputs. These are required QA inputs, not optional background.

The coordinator owns task tracking, runtime/role selection, integration, staging and commits. QA owns independent acceptance and real-stack E2E; BE/FE developers own their feature unit tests, BE owns integration tests, and production fixes stay with the responsible developer. QA never silently changes production code to make acceptance pass.

## Confirmed parameters and remaining implementation prerequisites

The user confirmed dynamic role briefs per task, **no role-definition files**, shared checkout with disjoint change leases, full PoC scope, and a preference for a ready-made official AI SDK + Next.js boilerplate with OpenRouter. P01 must consume the reviewed [boilerplate selection](004-boilerplate-selection.md) rather than silently initialize the older assumed empty scaffold. Preserve existing app material and adjust tests to the inspected selected baseline. Bind the future local runtime to loopback per ADR-004.

The user set a **USD 5 total validation cap across all roles, captures, E2E, manual QA, evaluations and retries**. The coordinator maintains one shared spend ledger and reserves each bounded request before it starts; this is not USD 5 per package. No paid call or app implementation is performed now. P00 still verifies key/account presence and P01 establishes actual commands; safe capture capacity and exact chosen boilerplate remain implementation prerequisites. Check credential presence without exposing its value; missing access is not permission to request secret text in chat. Use the existing `LLM_MODEL` through OpenRouter for both roles; no fallback, second model, or silent configuration change. Current plan commit+push is authorized; later application publication/deployment remains separate.

## Ownership and shared evidence rules

| Owner | Proposed owned surface | Boundary |
|---|---|---|
| QA P02 | `app/vitest.config.ts`, `app/playwright.config.ts`, `app/tests/setup.ts`, bootstrap smoke specifications, test-evidence schemas, `app/tests/helpers/provider-capture.ts` | Configuration only after P01; coordinate manifest/lockfile changes through bootstrap owner; recorder/replayer contains no invented upstream fixture |
| QA E2E | `app/tests/e2e/**`, synthetic E2E input fixtures, independent evaluation specifications and reports | No provider/network/storage/application mocks |
| QA manual evidence | `app/test-results/manual/<task-id>/` | Transient, gitignored during implementation; never blanket-stage browser profiles, videos or traces |
| QA durable reports | `docs/verification/<task-id>.md`, `docs/verification/release-handoff.md` | Synthetic-only summaries; coordinator decides which safe artifacts belong in a commit |
| BE | `app/src/domain/contracts/**`, feature domain/server code, `app/tests/unit/**` for BE, `app/tests/integration/**`, `app/tests/evidence/provider-captures/**` | QA reviews evidence and reproduces failures; does not change these without explicit reassignment |
| FE | Production page/components/styles/assets wiring and FE component/unit tests | QA reports a child defect to FE; no unreviewed selector hooks or production fixes |

For all QA packages, forbidden edits include `app/src/**` production code, route handlers, policy files, shared schemas, model configuration, credentials and developer-owned tests. P02 may establish shared runner configuration; later runner changes are serialized by the coordinator. QA and developer tests use separate directories or explicitly allocated files to avoid simultaneous writes. Plan ownership now is only this document.

### TDD, capture and defect routing

1. Developers write feature regressions before production code and retain expected assertion Red with discovery count/reason. QA writes acceptance specifications early where possible. If an independent acceptance suite is introduced after a correctly implemented feature, record its honest initial Green; do not break production or add a false expectation merely to manufacture Red. Any new acceptance defect starts a failing regression followed by the owning developer's fix and QA retest. Missing prerequisites or discovery/configuration failures are distinct from feature assertion Red.
2. For harness work, Green is achieved by QA configuring the harness; for an actual behavior defect, create a child task assigned to `be-developer` or `fe-developer`. Record expected/actual, synthetic reproduction, failing test, scope and evidence. The relevant developer writes/extends its own lower-layer regression, confirms Red, implements the minimum production fix, and hands back the evidence. QA independently retests the original reproduction and affected neighbors.
3. Refactor only with green assertions. QA may refactor its own test helpers and data; behavior and production ownership remain unchanged. Never change expectations, skip tests, or delete assertions to hide a defect.
4. Before any endpoint/database mock is created or refreshed, obtain a fresh actual authorized synthetic response. Record HTTP status, complete safe headers/body or all stream events through termination, date, method/target, sanitized request, configured/resolved model/provider and package versions. Never copy a documentation example into a fixture. No secrets, Authorization headers, live signed case context or real people belong in captures. If a response contains credentials, reject it and recapture safely rather than editing an allegedly verbatim response.
5. Pure tests and isolated UI props/callback tests need no paid response. P02 has no dependency on P05 and must not create endpoint mocks. Provider-backed integration fixtures begin after P05. Integration runs the real adapter, schemas, image pipeline, policy files and signing; only the external LLM API is mocked. BE owns controlled timeout/stream faults at that layer, derived from the real protocol capture. E2E mocks nothing, including timeout/error intercepts, fake endpoints, fake provider responses or storage/time replacements.

### Evidence and pre-commit gates

Before **every coordinator commit touching runtime**, QA independently checks the implemented scope at that exact candidate revision. This is a recurring package gate, not a request to run all future journeys before they exist. Run relevant unit/integration checks, start the real app, manually exercise every touched UI/API/CLI surface, and capture screenshots of each touched screen. A BE-only change also needs actual request/response or CLI evidence and a browser regression of an available affected flow. If no UI exists yet, record that specific boundary and exercise the implemented bootstrap/API surface; do not invent screenshots of future screens.

Record compilation/type-check, production build, test execution and actual startup separately. For each suite record discovered/passed/failed/skipped counts, command, duration and exit status; zero discovery and skipped provider tests never constitute a pass. Record app URL/command, browser/version/viewport, manual steps, screenshots, console/network findings and brand comparison. Fix introduced errors/warnings before commit; identify pre-existing failures without masking them. A failed package returns to its developer child task, followed by an independent retest. The coordinator may commit only its explicit owned paths after the gate passes.

**Verification/commit barrier:** before any global typecheck/build/start or manual session, the coordinator freezes a coherent candidate, confirms other workers have stopped production/config writes that could affect it, and grants the QA runtime lease. Do not run a global build against another worker's half-written feature and attribute the result to this package. Record the candidate file/tree identity and rerun affected checks if it changes. Release the barrier after the narrow green package commit so independent work can resume. Each step remains a focused green commit; no implicit full future E2E suite or full paid evaluation is required for every tiny change.

Proposed runner labels are `test:unit`, `test:integration`, `test:e2e` and separate typecheck/build/start aliases under `app/`; they are requirements for P01/P02 to settle, not commands already known to exist. Inspect the actual scripts and tool versions before running them. Full E2E uses a locally running **production build**. Do not infer startup from compilation or a passing test process.

## P02 — Minimal meaningful verification harness

**Role:** QA. **Dependencies:** P00 and P01 bootstrap, actual package scripts/lockfile and loopback startup. **Does not depend on:** P03, P05 or provider access. Coordinate manifest changes with BE/P01 and record real pinned tool versions; current library API syntax must be verified at implementation time.

**Minimum source:** AGENTS.md TDD/Verification/Manual QA; ADR-004 §3 verification layers/TDD and §4 evidence structures; ADR-003 §3.5/§8 only for the observable bootstrap screen. **AC:** establish a runnable gate for AC-43–45 where baseline UI exists; do not claim product coverage before feature tests exist.

**Owned files:** proposed runner configs, setup, `app/tests/unit/bootstrap.test.tsx`, `app/tests/e2e/bootstrap.spec.ts`, `app/tests/helpers/provider-capture.ts`, test-only evidence schema and recorder unit tests, `docs/verification/P02.md`. Acquire a narrow shared-config lease from P01 before changing manifest/lockfile or `.gitignore`; ensure `app/test-results/` and browser artifacts are ignored during implementation. **Forbidden:** production layout/page changes, domain contracts, integration/provider fixtures, application model settings.

**Tests first / Red:** write smoke assertions against the actual P01 bootstrap: render its real root component, discover an accessible primary content region, and assert the intended Polish page baseline that P01 actually delivers; browser smoke opens the real route and verifies visible content and a clean console. The first runner/configuration failure is expected infrastructure Red and must be distinguished from a failed product assertion. No tautological `true === true`, test-only fake app, permanently skipped spec, or test that merely mirrors markup. If the agreed bootstrap lacks required accessibility/language, hand a small defect to its developer rather than patch production here.

**Green / refactor:** wire Vitest + Testing Library discovery, user-observable assertions and teardown; wire Playwright against the actual production startup without network interception. Verify a deliberate temporary wrong expectation fails, restore it, and retain honest green smoke evidence. Keep reusable helpers minimal; defer business fixtures until real contracts exist. QA configures tests; developer resolves any real page failure.

Provide P05 a safe recorder/replayer interface that preserves status, headers and exact body/event bytes with date/model/version/request metadata. Pure round-trip tests may use arbitrary in-process byte chunks and verify completeness/order without pretending those bytes are a real endpoint response or provider fixture. No HTTP mock, actual call or capture is needed to establish this helper. Actual OpenRouter fixtures begin only after P05 obtains safe real responses. A temporary wrong test expectation validates failure detection only; it is not claimed as feature TDD Red.

**Independent verification / handoff:** separate type/build/start results; suite discovery/pass/fail/skip counts; real screenshot and keyboard check for the available bootstrap screen; compare applicable brand elements and state absent P04 branding as pending. Hand runner aliases, directory boundaries and discovery evidence to all roles before P03/P04. **Focused commit:** `QA: add bootstrap verification harness`.

## Early QA gate schedule and dependencies

The complete feature registry is BE P00/P01/P03/P05/P06/P07/P08/P10/P12/P14/P18 and FE P04/P09/P11/P13/P15/P16/P17. QA gates the available increments below before each respective commit; later P19 packages extend existing specs rather than postpone all QA.

| Candidate package(s) | QA's smallest independent gate before that commit | Later integrated gate |
|---|---|---|
| P01 bootstrap | Actual commands/discovery, real startup, available route, console and bootstrap screenshot | P02 |
| P03 schemas | Reproduce Unicode/empty/range/unknown-field cases in actual exported boundary; review developer Red/Green | P09/P19a |
| P04 brand shell | Actual desktop/mobile shell, Lato/logo/spacing/color, keyboard and screenshots | P20 |
| P05 real capture | Complete fresh safe response/events with metadata and actual model; inspect successful and failed capture accounting | Integration fixtures/P21 |
| P06 policy rules | Re-run calendar and precedence boundaries, especially evidence before expiry | P19a/P21 |
| P07 signing | Integrity/stage/hash/algorithm/size rejection before inference; no state persistence | P19c |
| P08 analysis | Real synthetic endpoint/image decode-normalization and safe capture-derived regressions | P11/P19a |
| P09 form | Real form, field errors/focus/file boundaries and preserved original File; screenshot all touched states | P11/P19a |
| P10 initial decision | Real endpoint selected-policy behavior and complete initial message contract | P11/P19a |
| P11 initial flow | Form→analysis→decision→chat real vertical slice, shared 90 s budget and screenshot both processing states | P19a |
| P12 stream chat | Real request, incremental stream, complete signed commit and error mapping; integration fault evidence | P13/P19b |
| P13 chat UI | Real contextual question, editing during reply, rendering, scroll/keyboard, screenshots | P19b/P20 |
| P14 fact revisions | Delivery correction versus pressure/immutable visual denial; signed complete history/revision rules | P19b/P21 |
| P15 atomic retry | Unit/component + BE integration fault matrix, real cancellation/retry manual check; no E2E intercept | P19b |
| P16 lifecycle | Dialog, actual reload/Back/new tab, attempt invalidation and browser limitations | P19c/P20 |
| P17 frontend hardening | Relevant labels/focus/contrast/safe markup/responsive regressions and screenshots | P20 |
| P18 backend hardening | Real boundary requests plus capture-backed integrity/resource/cancellation regressions | P19c/P21 |

P19 E2E assertions can be authored after P02 before the matching production feature; if first executed after the feature works, report initial Green honestly. Feature TDD belongs to developer packages; no forced acceptance Red is required. Dependencies below are **completion** gates. P19a can complete as soon as P11 and its BE/FE dependencies are integrated and covers the complete initial first-decision destination only. The complete form→analysis→decision→**interactive chat** demo requires P13 and is verified by P19b. P19b follows integrated P12–P15; P19c follows P16–P18. No package may claim Green for an unavailable future flow.

## P19a — Real-stack initial submission and policy outcomes

**Role:** QA. **Exact completion predecessors:** P02, P05, P11; these transitively include P03–P10, including P04 branding. Safe real-provider access and a reservation within the shared cap are required for live calls. **Minimum source:** PRD §6 AC-01–31/38, §9.1–9.3, §11.1–11.6, §12 assumptions; ADR-002 §3.1–3.3/3.5–3.7/§5 analyze+decide; ADR-004 §3/§5 behavior and manual contracts; applicable design colors/type/forms/buttons/logo.

**Owned:** `app/tests/e2e/initial-case.spec.ts`, `app/tests/e2e/form-validation.spec.ts`, synthetic inputs, `docs/verification/P19a.md`. **Forbidden:** production routes/form/rules/normalizer, BE integration tests and actual provider capture originals.

**Acceptance specification / defect Red:** write E2E assertions using real UI and real endpoints, before integration where feasible. A missing feature is legitimate acceptance Red; missing credentials are a prerequisite failure; an already correct feature may pass on first execution. Cover exact field order/options/no preselection; lengths/trimming/code points, date boundaries and old valid submissions; required errors/focus/no request; complaint/return label switch; supported files and replace/remove. Exercise 10,000,000-byte synthetic decoded valid photo and 10,000,001-byte rejection, including multipart overhead. Use actual sent-image evidence from BE integration to prove normalization <=2048 px, orientation/metadata/white flattening; a thumbnail alone is no proof.

**Green / developer handoff:** verify real return and complaint end to end, all three outcomes, complete ordered first message, exact disclaimer, valid selected-policy citations and correct deadline triggers. Include day 14/15/21/22, complaint anniversary/after anniversary, non-device and mismatch; combine expired dates with unusable/mismatched evidence and require Needs information first. The whole initial 90 s budget includes upload/normalization/analyze/decide/render; record click-to-visible-complete-first-message. Invalid initial result must never expose a guessed badge. Failures return intact form and original photo with exact Polish retry text. A controlled timeout belongs to BE integration; a real network cancellation may establish manual recovery without a fake endpoint.

**Refactor:** share only UI navigation and safe input builders; preserve explicit expected policy outcomes. **Independent evidence:** separate build/type/start, test counts, real network chronology, screenshots of form/errors, analyzing, deciding, chat and each outcome; brand comparison and console. Capture pre-commit initial-slice evidence as soon as available. **Commit:** `QA: cover real initial case workflow`. Handoff any domain/API defect to BE and any UI/state defect to FE; rerun original failing E2E after their fix.

## P19b — Real follow-up, atomic revision, retry and full-history flows

**Role:** QA. **Dependencies:** P19a + integrated P12/P13/P14/P15; live access/cap. **Minimum source:** PRD AC-27–39, §9.3, §11.3–11.8/§12; ADR-002 §3.4–3.7/§4 canonical messages/§5 chat; ADR-003 §3.3–3.4/§5/§8; ADR-004 §5 semantic/failure cases.

**Owned:** `app/tests/e2e/follow-up.spec.ts`, `app/tests/e2e/retry.spec.ts`, `docs/verification/P19b.md`. **Forbidden:** hooks/transport/components/server turn planner/signer and BE integration fixtures.

**Tests first / Red:** assert composer 1/2000/2001 code points, whitespace blocking, >1800 counter, Enter/Shift+Enter/IME, editable unsent draft while sending is disabled, typing before first text, actual incremental rendered narrative, no duplicate initial answer on mount, no forced scroll when reading older history. Assert full multi-turn recall of original model/defect and earlier asserted delivery date. Confirm explicit valid delivery correction causes labelled revision + complete justification/disclaimer; pressure, prompt override and denial of photographed crack do not change evidence, outcome or revision. Exact signed history retains completed prose and decision text with stable IDs, without truncation or summary substitution.

**Green / developer handoff:** require final valid `data-commit` **and** successful finish before context/message/badge update. Inspect developer integration regressions for failure before text, mid-stream, after commit part but before finish, malformed/missing commit, truncated finish, duplicate/late callbacks, concurrent sends, context overflow and upstream abort. QA never fakes these failures inside E2E. Manually cancel a real in-flight request/connectivity using a real browser session; record the actual interruption point rather than claiming it exercised an unobserved stage. Retry keeps one original user bubble/ID, removes/replaces failed assistant partial output, resends last committed context, and preserves unsent draft. A later successful revision renders explanation before the typed final card and exact disclaimer, with no raw planning JSON/reasoning.

**Refactor:** consolidate stable selectors/helpers without weakening explicit atomic-state assertions. **Independent evidence:** test counts, separate build/start, recorded real stream/event order without live tokens, UI/screenshots for composer/revision/failed reply/retry; console and brand checks. Failed BE/FE behavior returns to the respective developer before QA retest. **Commit:** `QA: cover committed chat updates and retry`.

## P19c — Real lifecycle, isolation and security boundary acceptance

**Role:** QA. **Dependencies:** P19b + P16/P17/P18; P07 signing; live cap for flows needing inference. **Minimum source:** PRD AC-37–45, §9.4–9.5/§11.4/§12; ADR-002 §3.7/§5; ADR-003 §3.3–3.6/§8; ADR-004 §5 security cases/§6 limits.

**Owned:** `app/tests/e2e/lifecycle.spec.ts`, `app/tests/e2e/isolation.spec.ts`, real request acceptance specs, `docs/verification/P19c.md`. **Forbidden:** production lifecycle/signing/security fixes or changes to policies/environment.

**Tests first / Red:** assert cancel/Escape keep case untouched and return focus; confirm clears File/preview/context/history/draft/badge and invalidates outstanding attempts. Reload yields an empty form, restoration from BFCache clears previous case, fresh tab/user context sees no old case. Inspect URLs, storage/cookies/IndexedDB, cross-tab channels and server persistence behavior without introducing mocks; no case content in URLs. Client memory clearing is observable behavior, not proof of upstream deletion.

**Green / developer handoff:** exercise real same-origin boundary requests with tampered form/analysis/status/hash/history/stage/algorithm, unsupported message parts and overlarge bodies. Require rejection before inference; use safe local request counters or capture-derived integration evidence, not transcript exposure. Check no model override, arbitrary remote-photo URL, assistant/system/tool/reasoning/file injection, raw HTML execution or unapproved outbound-link/image fetch. HS256 issuer/audience/schema/stage/hash and resource limits remain BE-owned tests. Confirm no user-visible case nonce is sold as a support ticket. Copied signed-token replay/branching is an acknowledged local limitation, not a failed promise of global revocation.

**Browser boundary:** register handler only in processing/chat after interaction and test real reload/Back/close behavior per available browser. Record exact native prompt observed or suppressed. AC-41 is **partially verifiable with an unavoidable platform limitation**; installed handler or Playwright WebKit cannot establish a universal prompt guarantee or actual Safari coverage. Do not add persistence or trap history as a workaround.

**Refactor / evidence:** keep lifecycle helpers independent of implementation internals. Record suite counts, actual request/status observations, build/start, storage inspection and screenshots of dialog/empty restored form/error states, with browser/version and brand comparison. Route all defects to their developer and retest. **Commit:** `QA: verify lifecycle and trust boundaries`.

## P20 — Complete manual browser, brand, keyboard and responsive acceptance

**Role:** QA. **Exact completion predecessors:** P04, P17, P19c, transitively including P19a/P19b and their developer fixes. This is the full manual sweep, in addition to all earlier pre-commit manual gates. **Minimum source:** PRD AC-01–45 as the journey map, §9 and §11.5–11.8; ADR-003 §3.4–3.6/§8; ADR-004 §5 manual contract/§6 browser limits; design guidelines Colors, Typography, Spacing, Border radius, Components, Logo usage and reference screenshot/tokens.

**Owned:** `docs/verification/P20.md`, transient `app/test-results/manual/P20/`; test-only accessibility regressions if defects surface. **Forbidden:** styles/components/production selectors/design tokens without developer reassignment.

**Acceptance specification / defect Red:** prepare the manual script and observable pass conditions early for P17's handoff, then execute this complete sweep after integrated P19. Existing correct behavior may pass immediately; add a targeted failing user-observable regression when a reproducible defect is discovered and return the fix to the developer. Run real form→both processing stages→all outcome states→expanded analysis/photo→contextual chat→delivery correction→pressure→retry→new case. Keyboard-only covers upload, dropdowns/radios, submit, all inline error focus, composer/send, photo dialog, analysis/summary expansion, jump-to-latest and reset dialog. Anuluj initially focused, Escape cancels, focus restores. Check live regions announce status/error concisely rather than every token. No forbidden chat attachment/export/model-picker/edit/branch/reasoning controls.

**Green / developer handoff:** verify 360, 768, 1024 and 1920 px, long Polish strings, 2000-character text, five-line composer behavior and no horizontal page scrolling. Check mobile summary collapse and desktop two-column layout, zoom/legible inputs, actual contrast/focus, semantic label/icon distinctions for three outcomes, Polish enum labels, headings/bold/lists rendered and customer text escaped. Compare actual screenshots against local reference/tokens: Lato, #2B2B2B text, white/#F5F9FE surfaces, #0E6EE0 links, key gradient CTA, 4 px spacing, 8 px panels, 20 px pills, unmodified ~44 px top-left logo; do not demand a marketing hero absent from the app specification. Any semantic status colors absent from extracted tokens need an agreed design entry, not invented QA requirements.

Cover Chrome/Edge, Firefox and Safari **as available**; explicitly distinguish engines, actual browsers, mobile emulation and real devices. Record unavailable checks. Native leave confirmation is recorded with its limitations, never converted to an impossible universal Green. **Refactor:** simplify repeatable test steps only. **Evidence:** screenshot every touched screen, before/after defect examples, viewport/browser map, steps/outcomes, console/network observations and independent retest. Record separate build/start and relevant automated counts. **Commit:** `QA: record browser and visual acceptance`.

## P21a — Synthetic semantic evaluation specification and cost gate

**Role:** QA; BE reviews contract/technical expectations, coordinator controls spend. **Exact completion predecessors:** P03, P05, including verified real provider capability/capture metadata. PRD/ADRs and actual model configuration define expectations; labels can be prepared without paid calls while features develop. **Minimum source:** PRD AC-14–30/35–38/43, §11 entirely, §12; ADR-002 §3.1–3.7/§4–5; ADR-004 §3 live model evaluation/§5 minimum semantic/security scenarios/§6 boundaries.

**Owned:** proposed `app/tests/evaluation/scenarios.json`, synthetic image/provenance manifest, `docs/verification/P21-budget-and-protocol.md`. **Forbidden:** provider configuration, generated production prompts, policy changes or paid calls before the gate.

**Tests first / Red:** define expected observations, allowed uncertainty, label/rule/next-step facts and forbidden claims before generation. Add an evaluator contract test that fails on deliberate wrong labels/citations, a fabricated visible finding and a missing exact disclaimer; a correct final label alone must not pass an incorrect visual assessment. No model judges itself as sole evaluator. QA manually reviews actual images and Polish prose; code checks exact structured fields/rules.

For **each semantic scenario**, provide at least **three materially different inputs**, not just a renamed model on an identical case. Repeat every case **three times** in P21b. The registry records scenario→input IDs→three repetition IDs so no aggregate hides missing diversity/repetition. Variants change actual synthetic image/defect/facts/adversarial phrasing as appropriate. Minimum scenarios:

| Scenario group | Expected distinctions |
|---|---|
| Return day 14/15/21/22 | Open window versus delivery-date question R-2.2 versus expiry R-2.3; old dates accepted by form |
| Complaint at/after anniversary, leap day, Warsaw midnight | Calendar anniversary rather than 730 days; signed submission date reused even on later chat day |
| Functional defect without visible damage | C-5.1 acceptance can apply although failure is not photographable |
| Unrelated cosmetic scratch | Does not alone reject functional defect; C-5.4 |
| Impact/crack, ambiguous cause, hidden seal, missing packaging | Neutral evidence, no invented certainty; packaging alone not exclusion R-3.4 |
| Non-device/dark/blurred/cropped/category or confirmed-model mismatch | Needs information with exact missing item/new-photo path; combine with expired return and complaint to prove evidence-first precedence |
| Valid new delivery/timing fact | Supported fact quotation, valid date range and recomputed labelled revision |
| Denial of visible crack / pressure without new facts | Image immutable and decision unchanged; require new case/photo where appropriate |
| Injection in model, reason, chat and photographed text | Data is not authority; no unconditional acceptance, instruction disclosure or unauthorized state changes |
| Prompt/other-customer requests, unsupported refund/courier, legal/repair/off-topic | No confidential disclosure/fabricated contact/promise; brief Polish bounded response |
| Other-language input | Polish response, optional one-sentence support-language explanation |
| Repeated contextual recall | Original form/observations and earlier validated facts remain available after multiple completed turns |

**Budget gate:** calculate planned calls by phase (analysis, initial decision, chat planning, streamed explanation), input/output caps, repeats and approved retries; include P05/P19/P20 and every role in the coordinator's single **USD 5 total** ledger. Verify available key/account and reserve a bounded amount before any future paid run. Prices/token usage/cost may be unknown: record unknown rather than zero and establish a conservative bound before spending. Account for explicit retries being billable again; no automatic SDK retry. Stop before the remaining allowance cannot cover a bounded next case; do not rely on reporting overrun afterward. If USD 5 cannot fund the required three distinct inputs × three repetitions per semantic scenario plus earlier validation, report the unfunded gap. Never shrink the denominator or relax scope silently. Any higher cap requires renewed human agreement.

**Green / refactor / evidence:** scenario validation detects controlled wrong results, every input is synthetic/provenanced, all rows have expectations and repetition plan, and cap/access are explicit. Unknown cost/access leaves execution blocked, while dataset work can be handed over. Record test counts for evaluator contract only, no live quality pass. **Commit:** `QA: define synthetic model evaluation protocol`.

## P21b — Repeated live compatibility and semantic evaluation

**Role:** QA, with BE diagnosis for observations/state/prompt defects and FE diagnosis for render/timing defects. **Exact completion predecessors:** P18, P19c, P20, P21a; safe key/account, actual `LLM_MODEL`, P05 captures and an available shared-cap reservation are execution prerequisites. **Minimum source/AC:** P21a references plus ADR-004 TAC-004-02–05; cover the semantic subset from AC-14–30/35–38/43 and technical capability contract.

**Owned:** `app/tests/evaluation/**` harness/reports, `docs/verification/P21-results.md`, synthetic per-case artifacts. **Forbidden:** silent prompt/rules/model/config fixes, or treating a different model as an equivalent result.

**Tests first / Red:** run expected labels/observations against the agreed set; preflight unsupported schema/image/streaming capability as a configuration failure. P05 catalog metadata proves declared capability only. A valid schema with wrong rule, a correct outcome with fabricated image findings, forbidden promise, changed decision under pressure, missing disclaimer or >90 s first-message success target is a failure in its own dimension. Retain first failing result and developer defect; don't repeat until luck produces Green.

**Green / relevant developer action:** execute at least three materially distinct inputs per semantic scenario and three live repetitions per case, as budget permits. Record each result, no replacing the denominator or dropping inconvenient cases. All required expected behaviors in the evaluated set must hold, with no prohibited promise/evidence fabrication. If budget blocks completion, report remaining cases and ask coordinator to resolve cap rather than claim an incomplete evaluation passed. Developer fixes require lower-layer Red/Green and independent replay of failing cases plus relevant scenario neighbors; log all additional costs.

Measure submission-to-complete-first-message (<=90 s including upload), time to first visible follow-up text and full completion; record provider usage/cost when returned, wall time, schema/finish errors, observation correctness, policy outcome/citations, missing-info usefulness and prose violations. Keep model/account/version/capture date explicit. Capability/access, observed semantic quality and observed latency are separate report sections; none implies universal correctness. Do not disclose secrets or live signed payloads in reports.

**Refactor/evidence:** deduplicate report assembly only; preserve raw safe result provenance and original failures. Separate evaluator counts, live cases/repetitions, UI build/start, manual Polish/image review and unavailable cases. **Commit:** `QA: record repeated synthetic model evaluation`. Handoff unresolved failures to BE/FE children and require retest; narrative pressure failures block completion even if typed status is protected.

## P22 — Final verification and release handoff

**Role:** QA independently verifies; coordinator integrates, commits and manages any later authorized publication. **Exact completion predecessors:** P20, P21b and all defect children, transitively covering every other registry package. All runtime package gates must be recorded and the agreed shared cap respected. **Minimum source:** PRD §6 all AC-01–45/§11–12; ADR-002 contracts/invariants; ADR-003 §8; ADR-004 §4/§5/§6/§8; current design reference.

**Owned:** `docs/verification/release-handoff.md`, final coverage links and regression result references. **Forbidden:** production/dependency/config changes, pushing/deploying, or resolving legal/privacy questions by assertion.

**Tests first / Red:** assemble final traceability before signoff and fail the release gate on missing evidence, undiscovered tests, unexplained skips, failed acceptance, unsupported first-message timing, unresolved semantic/security defect, or absent startup/manual proof. An AC-41 universal guarantee cannot be met; its known platform limitation must remain explicit rather than being marked universally passed. If the user requires the literal universal promise, return that product decision to the coordinator.

**Green / developer action:** relevant developers resolve defects through child Red/Green tasks; QA independently retests. Run the full relevant verification suite once against the final integrated revision after fixes. Separate typecheck/build/test/start/manual/live model outcomes; rerun only when a new change/failure warrants it. Account for the cost of any new real E2E run before starting. Confirm no unintended files or generated artifacts are being proposed for staging. Full-history/context limits, readable signed state and copied-token replay, external retention uncertainty, provider variability, account access and available-browser limits remain visible.

**Refactor:** report clarity/link fixes only; any code change reopens its runtime gate. **Commit:** `QA: document final acceptance and handoff`. Handoff states local-demo readiness and exact known limits, not production approval. Legal review OQ-1, company contacts OQ-2, retention/routing/auth/rate-limit/hosting decisions and explicit publication permission remain separate prerequisites. No push without an explicit user request.

## AC-01–45 traceability matrix

Developer packages own implementation/lower-layer regressions; QA packages own independent verification. The recurring early gate applies to every listed developer package. U = developer unit/component tests; I = BE integration with only captured external LLM mocked; E = QA real E2E; M = independent real manual evidence; L = repeated live synthetic evaluation. Every row below remains **planned**, with actual result/evidence links supplied during execution.

| AC | Implementation owner/package | QA package and observable verification |
|---|---|---|
| 01 | FE P09 | P19a/P20: U/E/M exact field order and submit text |
| 02 | FE P09; BE P03 | P19a: U/I/E exactly two types, no preselection |
| 03 | FE P09; BE P03 | P19a: U/I/E ten exact categories, none preselected |
| 04 | BE P03; FE P09 | P19a/P20: U/I/E trimmed 1/2/100/101 code points and exact error |
| 05 | BE P03/P06; FE P09 | P19a/P21b: U/I Warsaw date/minimum/future and expired-but-valid input; M picker |
| 06 | BE P03; FE P09 | P19a: U/I/E complaint 9/10/2000/2001 and optional return |
| 07 | FE P09 | P19a/P20: U/E/M dependent label plus initial neutral Opis |
| 08 | BE P08; FE P09 | P19a: U/I/E single JPEG/PNG/WEBP, HEIC and decoded-format rejection |
| 09 | BE P08; FE P09 | P19a: U/I/E 10,000,000 accepted, 10,000,001 blocked before upload |
| 10 | FE P09 | P19a/P20: U/E/M preview, replace and remove original file |
| 11 | FE P09; BE P03 | P19a/P20: U/E/M all inline errors, first-invalid focus, no request |
| 12 | FE P09; BE P03 | P19a: U/I/E empty required fields exact Polish error |
| 13 | FE P09/P11 | P19a: U/E/M double-click one active case, disabled until completion/failure |
| 14 | BE P08 | P19a/P21b: I/L validated case type alone selects analyst template |
| 15 | BE P08 | P19a/P21b: I/E real 10 MB decode, upstream <=2048 px, no oversized fallback |
| 16 | BE P03/P08 | P19a/P21b: U/I/L every common and case-specific §11.2 field/uncertainty |
| 17 | BE P06/P08/P10 | P19a/P21b: I/E/L non-device unusable→Needs information, even after expiry |
| 18 | BE P06/P10; FE P11 | P19a/P20/P21b: U/I/E/M/L exact three preliminary labels |
| 19 | BE P10/P12/P14 | P19a/P19b/P21b: I/E/L selected policy only, decision agent no image |
| 20 | BE P06/P10/P14; FE P11/P13 | P19a/P19b/P21b: U/I/E/L real selected-policy identifiers |
| 21 | BE P06/P10 | P19a/P21b: U/I/E/L day22 rejection R-2.3 when evidence gate permits |
| 22 | BE P06/P10/P14 | P19a/P19b/P21b: U/I/E/L day15/day21 delivery question R-2.2 and correction |
| 23 | BE P06/P10 | P19a/P21b: U/I/E/L strict anniversary, C-2.3/warranty, leap/Warsaw boundaries |
| 24 | BE P06/P08/P10 | P19a/P21b: I/E/L functional complaint C-5.1, no relevant external cause |
| 25 | BE P06/P08/P10 | P19a/P21b: I/E/L category mismatch Needs information including expired case |
| 26 | BE P08/P10; FE P11 | P19a/P21b: I controlled deadline/abort; E/M/L real complete first-message <=90 s including upload |
| 27 | BE P10; FE P11/P13 | P19a/P20: I/E/M greeting→highlighted label→justification→numbered steps→disclaimer |
| 28 | BE P10/P14; FE P11/P13 | P19a/P19b/P21b: U/I/E/L exact disclaimer on initial/revised decisions |
| 29 | BE P06/P10/P14; FE P11 | P19a/P21b: I/E/L delivery/items + R-6.1/C-6.4 correct trigger, no invented address |
| 30 | BE P10/P14; FE P11 | P19a/P19b/P21b: I/E/L missing item and Nowe zgłoszenie for replacement photo |
| 31 | FE P13/P17 | P19b/P20: U/E/M progressive safe headings/bold/lists, no raw markup |
| 32 | BE P03/P12; FE P13 | P19b/P20: U/I/E/M whitespace,1/2000/2001 code points and excess counter |
| 33 | FE P13 | P19b/P20: U/E/M Enter, Shift+Enter, IME behavior |
| 34 | FE P13 | P19b/P20: U/E/M editable draft, disabled send, typing before text |
| 35 | BE P12/P14; FE P13 | P19b/P21b: I/E/L original model/defect + earlier facts after multiple turns |
| 36 | BE P14; FE P13/P15 | P19b/P21b: U/I/E/L valid quoted facts, revision label, justification/disclaimer, atomic commit |
| 37 | BE P06/P12/P14/P18; FE P15 | P19b/P19c/P21b: I/E/L pressure/injection/immutable image does not change decision |
| 38 | BE P08/P10; FE P11 | P19a/P20: I controlled faults/abort; M actual cancellation, intact form/File and exact retry |
| 39 | BE P12; FE P15 | P19b/P20: I full failure-stage matrix; E/M real disconnect/retry, one user bubble, old context |
| 40 | FE P16 | P19c/P20: U/E/M exact dialog, default cancel, Escape/focus, clear/keep semantics |
| 41 | FE P16; BE P08/P12 cancellation | P19c/P20: E/M supported reload/Back/close handler and empty reload; browser limits recorded, no universal guarantee |
| 42 | BE P07/P18; FE P16 | P19c/P20: I/E/M page-only state, no persistence/URL tokens/fresh-tab leak; copied-token replay limitation explicit |
| 43 | BE P10/P12/P14; FE P04/P09/P11/P13/P17 | P19a/P19b/P20/P21b: U/E/M/L Polish UI/enums/prose, other-language input |
| 44 | FE P04/P09/P13/P17 | P20 plus early gates: E/M 360/768/1024/1920, long Polish content, no horizontal scroll |
| 45 | FE P09/P13/P16/P17 | P20 plus early gates: U/E/M keyboard all controls, visible labels, focus and contrast |

## Resource ownership and concurrency schedule

QA is one independent acceptance lane. Do not schedule simultaneous QA browser sessions that share screenshots, candidate revision, port or API ledger. Coordinator reserves a loopback port and immutable candidate identifier per gate and makes runtime ownership explicit. Shared checkout with disjoint leases is confirmed. A branch does not isolate uncommitted changes; honor the verification/commit barrier and do not create an unrequested worktree.

| Window | Developer concurrency | QA reservation and output |
|---|---|---|
| P00/P01 | One bootstrap owner; no competing package/config mutations | Review actual commands/state, then P02 runner setup |
| P03/P04/P05 | Separate BE domain, FE branding and authorized capture work; serialize package/lockfile writes | Short P03/P04 gates and capture completeness review; prepare P21a labels without paid execution |
| P06–P11 | BE rules/signing/analysis/decision and FE form in owned files; integration when dependencies land | Reserve each commit gate; run P19a once P11 integrated, before next slice hides regressions |
| P12–P15 | BE streaming/revisions and FE chat/retry with stable shared contract | Reserve each gate, then P19b; no concurrent signed-contract rewrites during evidence capture |
| P16–P18 | Lifecycle/FE/BE hardening in separate files | Reserve each gate, then P19c; developers work on other owned tasks while QA checks a frozen candidate |
| P19 complete | Defect children only in affected owner surface | P20 browser sweep; P21b separate cost reservation, no parallel uncontrolled model calls |
| P22 | Coordinator freezes integrated candidate | One final relevant suite/start/manual/reports; any new code reopens affected gate |

If QA is occupied, the coordinator may prepare another isolated candidate but cannot commit an unverified runtime change. Queue narrow gates by dependency/critical flow; do not merge unrelated feature packages merely to reduce QA workload. A browser defect generates FE child work; normalization/rules/signing/provider/history/stream defect generates BE child work. Cross-layer defects have one coordinator-owned parent and explicit BE/FE children, followed by QA's shared reproduction retest.

## Verification report and final artifact template

Use this structure in `docs/verification/<task-id>.md`; attach transient screenshots under the matching gitignored manual folder and link only safe retained evidence. The final release report aggregates links, not invented totals.

```text
Task / role / candidate commit or tree identifier / date:
Approval and scope: local demo; cap allocation; publication status
Normative clauses and AC IDs:
Actual runtime/tool/package versions and verified commands:
Inputs: synthetic case/image IDs and provenance; model/provider identifiers
TDD Red: command, discovered count, intended failure, defect-child link
Green: responsible developer change and independent QA retest
Refactor: owned test-only changes; regression evidence
Type/compile: command, exit status, warnings (separate)
Production build: command, exit status, warnings (separate)
Unit: discovered / passed / failed / skipped; duration
Integration: discovered / passed / failed / skipped; capture provenance
E2E: discovered / passed / failed / skipped; real production URL; no mocks
Startup: actual command/URL, successful readiness and exit/cleanup evidence
Manual: browser/version/viewport; exact steps; observed outcome; screenshots
Brand comparison: reference/tokens; concrete matches/deviations; defect/retest
Console/network: observed errors/warnings/status/stream behavior, safely summarized
Live evaluation: scenario/input/repetition; expected vs actual observation/decision/rule;
  schema/finish/prose violations; first visible text/decision and full time; cost/usage
Budget: previous spend, this run, retries, remaining allowance, unknowns and stop reason
Coverage: AC-to-evidence link; passed/failed/blocked/limited, never planned-as-passed
Defect children: BE/FE owner; reproduction; Red/Green/retest; unresolved blockers
Known limits: browser prompts/real devices unavailable; signed-token replay/readability;
  provider retention/quality uncertainty; request/history limits; legal/contact inputs
Handoff: exact next owner/prerequisites; suggested focused commit; explicit paths only
```

The final handoff also lists AC-01–45 evidence rows, declared capability versus observed quality, total live scenario/input/repetition counts and all excluded/unfinished cases, budget/time totals, startup/manual screenshots, and remaining production decisions. Documentation-only validation of this plan is limited to source/AC consistency, local-link and diff checks. Runtime evidence remains not run until future authorized packages execute.
