# ADR-001: Initialize the Empty Application

**Date:** 2026-10-18

**Status:** Accepted

**Relates to:** [Main architecture](000-main-architecture.md)

## 1. Scope

Define how the implementing agent creates the Next.js application itself. No initialized template, package manifest, route or test runner may be assumed. This record specifies initialization and verification contracts, not implementation code.

## 2. Technology Documentation References

| Library | Context7 ID or official docs | Used for |
|---|---|---|
| Next.js | `/vercel/next.js`; [create-next-app](https://nextjs.org/docs/app/api-reference/cli/create-next-app) | Official generator and supported options |
| Generator source | [Directory conflict check](https://github.com/vercel/next.js/blob/canary/packages/create-next-app/helpers/is-folder-empty.ts) | Why the current scaffold needs a staging directory |
| AI SDK | `/vercel/ai`; [v7 migration](https://ai-sdk.dev/docs/migration-guides/migration-guide-7-0) | Runtime and current API requirements |
| OpenRouter provider | [README](https://github.com/OpenRouterTeam/ai-sdk-provider) | Supported SDK release line |
| Package metadata | [npm registry](https://registry.npmjs.org/) | Exact release snapshots and peer/engine declarations |
| AI Elements | `/vercel/ai-elements` | Selective component generation |
| Sharp | `/lovell/sharp` | Node-side image normalization |
| JOSE | `/panva/jose` | Context signatures |

## 3. Component Design

### Reproducible baseline

The following releases were observed in public npm metadata on 2026-10-18. Use this coherent baseline for initialization, then record the installed versions in the lockfile. Recheck metadata and docs before intentionally changing a major version.

| Package/runtime | Baseline | Constraint |
|---|---|---|
| Node.js | 24.x | One supported major across laptops; SDK/provider require at least 22 |
| pnpm | 12.14.0 | Declare the package-manager version |
| `create-next-app`, `next` | 16.4.2 | Official stable generator, not canary |
| `ai` | 7.0.143 | Current v7 APIs and ESM |
| `@ai-sdk/react` | 4.0.147 | AI SDK 7 React release line |
| `@openrouter/ai-sdk-provider` | 3.1.0 | Declares `ai ^7.0.0` and Node >=22 |
| `jose` | 6.2.12 | Sign/verify trusted context |
| `vitest` | 5.0.5 | Supports Node 24 |
| `@playwright/test` | 1.64.1 | Browser verification baseline |

Use Zod 4 satisfying the SDK/provider peer requirement of at least 4.1.8. Select a matching React/React DOM pair satisfying the published Next.js and AI SDK React peer requirements; do not blindly retain the legacy donor pair. Select and lock Sharp, Tailwind, Testing Library and the generated UI dependencies during bootstrap; do not bypass incompatible peer declarations.

### Planning update — 2026-10-18: selected ready-made starter

The user's later instruction prefers a ready-made AI SDK + Next.js boilerplate. For scaffold acquisition only, this supersedes the earlier empty `create-next-app` choice. Use the official-team [vercel-labs/ai-sdk-gateway-demo at `6fb429b62f1ea8aaea5eee468723d92e61896fd8`](https://github.com/vercel-labs/ai-sdk-gateway-demo/tree/6fb429b62f1ea8aaea5eee468723d92e61896fd8), with the allow-list, Apache-2.0 provenance and comparison recorded in [boilerplate selection](../plans/004-boilerplate-selection.md). The historical release table remains research evidence; its generator row is no longer the acquisition procedure.

The source uses older Next.js/SDK releases. Reuse only reviewed infrastructure and selected shadcn primitives; remove Gateway/demo routes, model selection, theme persistence and remote fonts. Align its manifest to the accepted Node 24, Next.js 16.4.2, AI SDK 7/React bindings 4/OpenRouter provider 3 baseline **before installation**. Do not migrate the SDK 5 chat wholesale or claim compatibility before actual checks. Strict TypeScript, read-only metadata research, README preservation, no generated agent instructions, no extra auth/database/persistence, synthetic-only data and no inference during build remain unchanged. No application has been acquired, installed or run by this planning update.

### Initialization procedure

1. Inspect `app/` immediately before acquisition. Preserve `app/README.md` and all root/course documentation. Stop on unexpected application files rather than replacing them; verify the selected source revision and allow-list from the planning update.
2. Acquire that exact pinned source into a fresh temporary staging directory outside `app/`. Retain its licence/provenance; do not overlay a full repository or copy nested Git/agent configuration.
3. Prepare only the allow-listed infrastructure, Tailwind/shadcn configuration, selected primitives and utility. Remove demo/Gateway/model-picker/persistence/remote-font behavior. Adapt paths to App Router, `src/`, `@/*` and pnpm; pin the accepted dependency family before installing. Keep React Compiler and Cache Components disabled. No real provider behavior is added in bootstrap; domain routes follow their own tests-first packages and real-capture prerequisites.
4. Transfer the reviewed files by explicit path. Preserve the application README and integrate startup instructions only after commands exist. Do not overwrite repository instructions. Record retained/removed source files and adaptations; remove only this task's staging files after verification. If any nontrivial source behavior is retained and changed, characterize it before adaptation.
5. Install the coherent AI dependency set, Zod, JOSE and Sharp inside `app/`. Retain/adapt the existing `components.json` and CSS-variable configuration without rerunning shadcn initialization over it; P04 owns installation of only the AI Elements components selected in ADR-003 after P02, under the serialized package/configuration lease. Use pnpm tooling; do not install a second package manager or global generator.
6. Verify `strict: true` in the generated TypeScript configuration. Keep generated Next.js compiler settings. Add `noUncheckedIndexedAccess` for domain/boundary code and resolve resulting errors; prohibit unvalidated `any` at HTTP and model boundaries.
7. Add meaningful test infrastructure before production feature code: Vitest, DOM test setup and Playwright. Write failing behavior tests from the PRD before implementing each feature.
8. Create ignored `app/.env.local` from the public example variable names. Generate the separate signing secret locally. Extend an example configuration with its placeholder during implementation. Never copy secrets into committed examples.
9. Add explicit scripts for type checking, lint, unit/integration tests, E2E, build and startup. Next.js 16 does not provide `next lint`, and a successful build does not replace lint execution.
10. Verify a production build and start it locally; exercise the baseline screen. Record installed dependency versions and actual script outputs. Do not claim success from the generator's exit status alone.

The original generator research found that `app/README.md` makes the folder non-empty. The same preservation constraint applies to starter acquisition: staging plus an explicit allow-list avoids replacing the scaffold or teaching materials.

### Runtime and source assets

Start all application commands with `app/` as the working directory. The server policy loader resolves `../docs/policies/` from that known root and validates both required files. Load policy content only server-side; no policy path is accepted from HTTP input. Include both files in future deployment packaging if hosting is introduced.

Copy only the required logo, favicon and local Lato fonts from root `assets/` into `app/public/` during initialization, preserving their source attribution. Map design tokens into application CSS variables. Do not include the full homepage screenshot in the runtime bundle. Reuse local fonts rather than adding a build-time network dependency on a font service.

## 4. Data Structures

| Artifact | Contract |
|---|---|
| Package manifest | One package; ESM; Node 24 major; declared pnpm version; explicit scripts |
| Lockfile | Committed `app/pnpm-lock.yaml`; no competing npm/yarn lockfile |
| TypeScript configuration | Strict checking enabled; generated Next.js integration retained |
| Environment schema | Non-empty model/key and correctly sized signing key validated by server modules; no secret values in errors |
| Policy bundle | Both files plus version/hash; no request-time arbitrary path selection |
| UI component registry output | Owned source files and reviewed dependencies, not an opaque full-app template |

## 5. Interface Contracts

The implementing agent must establish these package scripts. They are future interface requirements, not commands verified against an existing application.

| Script | Required behavior |
|---|---|
| `pnpm dev` | Start the local development app on loopback |
| `pnpm typecheck` | TypeScript check without emit |
| `pnpm lint` | Execute ESLint directly |
| `pnpm test` | Deterministic non-watch unit/integration run; report discovered/passed/failed/skipped counts |
| `pnpm test:e2e` | Exercise a running local production app and the real provider; fail clearly when credentials are unavailable |
| `pnpm build` | Create the production Next.js build |
| `pnpm start` | Start the built app on loopback for manual QA |

No real OpenRouter call may occur during static generation or build. Missing runtime configuration must fail clearly at the server boundary before a customer request is sent upstream.

## 6. Technical Decisions

### Official minimal generator instead of a chat application fork

**Status:** Superseded for scaffold acquisition by the dated planning update above; retained as historical reasoning

**Date:** 2026-10-18

**Context:** The application is empty and most screens are case-specific. Existing chatbot templates introduce persistence and account flows.

**Decision:** Use the pinned official Next.js empty template in staging and add only the required integrations.

**Rejected alternatives:** Handwriting every framework configuration risks inconsistent defaults. Forking `vercel/chatbot` requires removing several integrated services.

**Consequences:** (+) Small starting surface and auditable initialization. (-) The agent must add the test harness and case-specific screens.

**Review trigger:** The PRD adds accounts, durable multi-thread history and stored attachments together.

### Current coherent SDK release family

**Status:** Accepted

**Date:** 2026-10-18

**Context:** Current documentation is v7; older examples remain online. The current provider explicitly supports v7.

**Decision:** Use AI SDK 7, React bindings 4 and provider 3 together, on Node 24. Prefer `instructions`, the `stream` result and top-level UI stream helpers. Use canonical file content parts for image bytes.

**Rejected alternatives:** Mixing SDK 7 with an older provider or copying v5/v6 integration examples can produce incorrect types and transport behavior.

**Consequences:** (+) Matches verified current patterns. (-) Registry-generated examples still need review for older API usage.

**Review trigger:** A dependency upgrade changes peer requirements or a selected component fails strict compilation.

## 7. Diagrams

### Initialization architecture and data flow

```mermaid
flowchart LR
    Sources[PRD, policies and brand assets] --> Inspect[Inspect current scaffold]
    Starter[Pinned reviewed starter] --> Stage[Temporary staging directory]
    Inspect --> Stage
    Stage --> Merge[Transfer allow-listed adapted paths]
    Merge --> Package[Application package under app]
    Sources --> Package
    Package --> Checks[Types, lint, tests, build and startup]
```

### Bootstrap sequence

```mermaid
sequenceDiagram
    participant Agent
    participant Scaffold as Existing repository
    participant Starter as Pinned starter source
    participant App as Adapted application
    Agent->>Scaffold: Inspect app and preserve README
    Agent->>Starter: Acquire exact source revision in staging
    Starter-->>Agent: Allow-listed infrastructure and primitives
    Agent->>App: Transfer explicit paths and install coherent dependencies
    Agent->>App: Establish tests before feature code
    Agent->>App: Check strict types, lint, tests and build
    alt Checks pass
        Agent->>App: Start and inspect real baseline UI
    else Any check fails
        App-->>Agent: Diagnostic to resolve before completion
    end
```

## 8. Testing Strategy

Bootstrap checks are configuration and observable startup checks. Do not add tests that merely mirror package contents. Feature tests begin with the real domain contracts in ADR-002 and the interaction requirements in ADR-003.

| Scenario | Type | Input | Expected output | Edge cases |
|---|---|---|---|---|
| Existing scaffold survives | Diff check | Current `app/README.md` and root documents | Preserved content, no nested Git | Unexpected application files stop transfer |
| Dependency compatibility | Install/type check | Pinned core set | One coherent dependency graph | No suppressed peer conflicts |
| Missing environment | Integration | Missing key/model/signing key | Configuration error without secret disclosure | Placeholder signing key rejected |
| Initial application | Build/start/manual | Installed local package | Root page renders and no console errors | Real fonts/assets resolve |

- **TAC-001-01:** The agent can initialize from the current README-only folder without deleting its contents.
- **TAC-001-02:** Strict type checking and ESLint run independently of production build.
- **TAC-001-03:** All future commands are verified from `app/`; no root package is assumed.
- **TAC-001-04:** No generated `AGENTS.md`, hook, nested repository or unrequested service is introduced.
- **TAC-001-05:** Build and startup do not depend on a live model call or remotely downloaded fonts.
