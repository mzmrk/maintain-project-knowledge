# Maintain Project Knowledge

Help coding agents carry project understanding across sessions.

This skill maintains a compact set of project documents grounded in the actual implementation. Agents read that knowledge
before working, verify the parts their task touches, and update it when something worth remembering changes.

It is designed for projects where repeatedly explaining architecture, rediscovering constraints, or correcting the same
mistakes is becoming expensive.

## Get started

For a personal Codex installation using the default skills directory:

```sh
git clone https://github.com/mzmrk/maintain-project-knowledge.git \
  ~/.codex/skills/maintain-project-knowledge
```

For one project, clone into `.agents/skills/maintain-project-knowledge/` instead. If you configured a different Codex home,
use its `skills/` directory.

Then invoke it with your task:

```text
Use $maintain-project-knowledge while adding CSV export.
```

Or ask it to review existing knowledge:

```text
Use $maintain-project-knowledge to check our docs against the implementation.
```

The workflow targets Linux and assumes Git, standard shell tools, and Node.js. The bundled validator has no package
dependencies; CI uses Node.js 22.

## What it preserves

Useful project knowledge explains things a future contributor would otherwise need to investigate:

- What the product supports and deliberately leaves outside its scope.
- Which components own behavior and how data moves between them.
- Contracts, invariants, and ordering requirements that changes must preserve.
- Persistence, deployment, privacy, and recovery constraints.
- Non-obvious decisions, their rationale, and limitations that still matter.

For example, discovering that two screens must persist settings independently should lead to a short, source-linked
explanation of that boundary. Temporary debugging notes and a narrative of the investigation should disappear.

Each substantive claim has one owner. Other documents link to that explanation instead of maintaining competing copies.

## How it works

1. **Load the context.** Read the complete core documentation in a fresh context.
2. **Understand the affected system.** Trace ownership, behavior, callers, contracts, and tests before changing or describing it.
3. **Resolve important uncertainty.** Make routine engineering decisions independently; ask when unresolved choices would materially change the intended outcome.
4. **Maintain useful knowledge.** Update durable facts, replace stale claims, and remove duplication.
5. **Verify.** Check documentation against source and run the relevant validation.

Implementation, schemas, configuration, and tests take precedence over documentation when describing current behavior.
Intended policy remains distinct from what is implemented.

When no durable knowledge changes, the skill leaves the documentation untouched.

## Compact memory, explicit boundaries

Project memory lives under `docs/`, indexed by `docs/README.md`.

The core has a **6,000-word limit**. Agents consolidate it before adding specialist detail to `docs/references/`. References
have explicit read conditions and must be verified before use. Optional raw evidence—such as an exact response or
screenshot—belongs under `docs/evidence/` and must be linked from its owning document.

The root README remains human-facing onboarding. It is reviewed when public instructions or capabilities change, rather
than loaded as routine agent memory.

Documentation maintenance alone does not authorize unrelated implementation changes.

## Where it helps—and what it costs

The skill is most useful for ongoing projects, repeated agent sessions, and systems with important constraints that are
easy to miss. It aims to reduce repeated investigation and preserve understanding as the code evolves.

That comes with tradeoffs:

- **Reading costs context and time.** Complete core loading can be disproportionate for tiny or disposable tasks.
- **Existing documentation may need restructuring.** The skill deliberately enforces a compact core and a particular references/evidence layout.
- **Accuracy still requires judgment.** Source links and validation help expose drift; they cannot guarantee truthful documentation or correct agent decisions.
- **It includes workflow opinions.** English Markdown, full core reads, and committing completed verified changes are defaults. Task and repository instructions take precedence.

Read [SKILL.md](SKILL.md) for the complete rules before adopting it.

## Validation

From your project root, with a project-local installation:

```sh
node .agents/skills/maintain-project-knowledge/scripts/validate-docs.mjs
```

The validator checks documentation structure, internal links, index coverage, word limits, and reference/evidence
ownership. Factual verification remains the agent’s responsibility.

To run the validator’s tests from this skill repository:

```sh
node --test scripts/validate-docs.test.mjs
```

## License

[MIT](LICENSE) © mzmrk.
