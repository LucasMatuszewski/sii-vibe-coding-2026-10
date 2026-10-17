Use the installed `write-adr` skill to document the architecture for the synthetic **Hardware Service Decision Copilot** described in `docs/PRD.md`. Read the whole PRD first. Follow the skill's clarification, current-documentation and ADR workflow; do not write code in this step. The PRD controls product behavior, and the ADR records technical decisions and their tradeoffs.

The course repo starts from an unsolved scaffold and may be used by people in different stacks. Ask which language, framework and local environment this group is actually using before selecting components. The instructor's example may use a web UI plus API, but a CLI-first implementation is also acceptable if it satisfies the same acceptance criterion. Do not copy a historical Next.js/Spring/Angular decision without validating it for this repo and the current official documentation.

Research the current official documentation for these candidate libraries and APIs before proposing the architecture. Compare their roles instead of assuming they are interchangeable:

- Vercel AI SDK: https://ai-sdk.dev/docs/introduction ; AI SDK UI useChat: https://ai-sdk.dev/docs/reference/ai-sdk-ui/use-chat ; structured output: https://ai-sdk.dev/docs/ai-sdk-core/generating-structured-data .
- Chat SDK: https://chat-sdk.dev/ . Check whether its multi-platform chat role fits this product, rather than assuming it is the browser chat UI layer.
- OpenRouter provider for the AI SDK: https://openrouter.ai/docs/guides/community/vercel-ai-sdk ; OpenRouter Responses API: https://openrouter.ai/docs/api/reference/responses/overview .
- Next.js App Router: https://nextjs.org/docs/app .

Resolve and record at least these decisions:

- Where form validation occurs and how field-level errors cross the UI/API boundary.
- How images are accepted, size-limited, temporarily stored or discarded, and represented in the model request. State how failures are shown to the user.
- How the return and complaint policy documents are loaded, versioned and cited by the recommendation. The synthetic policy files must be the only policy facts in the lab.
- How image analysis and decision generation are separated, including structured output validation, uncertain results and the human-review category.
- How the first recommendation, follow-up chat and any re-evaluation are related without losing the original outcome.
- How an external model is isolated behind a testable seam. Unit tests use deterministic fixtures; an optional live check is bounded and clearly separated from repeatable tests.
- How secrets and logs are handled. No key belongs in browser code, prompt examples, screenshots or Git history.

For each choice, describe an alternative and why it was rejected or deferred. Provide a component diagram, a sequence diagram for successful submission, a sequence diagram or table for failure/uncertainty, conceptual data structures, interface contracts, and a testing strategy tied to PRD acceptance criteria. List exact build/test/run commands for the chosen scaffold only after verifying them locally. Record the official documentation sources and versions used for every selected library. Keep a short list of unresolved decisions rather than guessing.

Write the ADR under `docs/ADR/` using the file layout the skill selects. Stop after the ADR and its verification; implementation is a separate exercise.
