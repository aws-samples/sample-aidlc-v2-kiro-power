# AI-DLC v2 Usage Guide

## Starting an AI-DLC Workflow

Once installed, use AI-DLC by starting your prompt with:

> "Using AI-DLC, ..."

For example:
- "Using AI-DLC, build a REST API for user management"
- "Using AI-DLC, create a React dashboard for analytics"
- "Using AI-DLC, design a microservices architecture for an e-commerce platform"
- "Using AI-DLC, add authentication to my Express app"

## How the Workflow Operates

The AI-DLC orchestrator activates automatically and proposes a workflow tailored to your intent. The typical flow is:

### Inception Phase (What & Why)

1. **Intent Bootstrap** — Captures your development intent and asks structured questions
2. **Requirements Analysis** — Generates formal requirements from your answers
3. **User Stories** — Creates user stories from requirements (conditional — complex user-facing features)

### Construction Phase (How)

4. **Functional Design** — Produces functional design artifacts (technology-agnostic business logic)
5. **Application Design** — Architecture and component design
6. **Infrastructure Design** — Infrastructure considerations
7. **NFR Assessment** — Non-functional requirements evaluation
8. **NFR Design** — NFR patterns and logical components
9. **Code Generation** — Deterministic code generation with verification
10. **Unit Generation** — Test generation for the produced code

### Adaptive Behavior

The workflow adapts to your project's complexity:

**Simple Bug Fix:**
- Intent Bootstrap → Requirements (minimal) → Code Generation → Build & Test

**New Feature in Existing App:**
- Intent Bootstrap → Reverse Engineering → Requirements → Functional Design → Code Generation → Build & Test

**Greenfield Application:**
- Intent Bootstrap → Requirements (comprehensive) → User Stories → Application Design → Units Generation → [Per-unit Construction] → Build & Test

## Key Principles

- **Human-in-the-loop**: You review and approve each artifact before the next stage runs
- **Deterministic verification**: The process checker validates outputs at each stage
- **Artifacts are external**: Generated docs land in `org-ai-kb/aidlc-docs/intent-<nnn>-<slug>/`
- **Code stays in project**: Generated source code goes into your project directory
- **Adaptive execution**: Only stages that add value are executed

## Prompt Behavior

After this power is installed, at the start of each new conversation you'll be asked once:

> "Would you like to use AI-DLC for this session, or proceed normally?"

Your choice applies for the entire conversation — you won't be asked again on subsequent prompts.

- **Choose AI-DLC** for new features, major changes, or when you want structured development
- **Choose normal** for quick fixes, questions, or simple edits

If you've already prefixed your message with "Using AI-DLC", the orchestrator activates directly without asking.

## Answering Structured Questions

The methodology uses structured questions to gather requirements:

```
Which approach do you prefer for user authentication?

A) OAuth 2.0 with third-party providers (Google, GitHub)
B) Traditional username/password with JWT tokens
C) Multi-factor authentication with email verification
D) Social login only (no traditional signup)
E) Other (please describe)

[Answer]: B
```

Use the `[Answer]:` tag followed by your letter choice.

## Artifact Output Structure

AI-DLC generates documentation in a structured folder:

```
org-ai-kb/aidlc-docs/intent-<nnn>-<slug>/
├── aidlc-state.md          # Workflow state tracking
├── audit.md                # Complete audit trail
├── inception/              # Planning phase artifacts
│   ├── requirements/
│   ├── application-design/
│   └── plans/
└── construction/           # Implementation phase artifacts
    ├── [unit-name]/
    │   ├── functional-design/
    │   ├── nfr-requirements/
    │   ├── nfr-design/
    │   └── code/
    └── plans/
```

## Tips

- Use Kiro in **Vibe mode** — decline if nudged to switch to Spec mode
- Be specific in your intent statement for better workflow routing
- Review each stage's output carefully before approving
- The orchestrator may skip stages that aren't relevant to your intent
- You can always ask to re-run a stage with different parameters
- Trust the adaptive assessment — simple changes will skip most conditional stages

## Reverse Engineering

For existing codebases, use:

> "Using AI-DLC, reverse-engineer the authentication module"

This activates the reverse-engineering skill to analyze and document existing code before making changes.

## Custom Workflow Composition

For advanced users, you can compose custom workflows:

> "Using AI-DLC, compose a workflow that skips wireframes and goes straight to code generation"

The workflow-composition skill lets you tailor the pipeline to your needs.

## Quality Gates

At each major stage, you'll be asked to:
- Review the generated plans and designs
- Approve proceeding to the next stage
- Request modifications if needed

The process-check hook ensures deterministic compliance at each transition.
