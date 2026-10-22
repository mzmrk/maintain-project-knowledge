# Maintain Project Knowledge

An agent skill for keeping project knowledge accurate, compact, and grounded in source code.

Install it in your coding agent, then invoke `$maintain-project-knowledge` while working on a repository. It helps the
agent understand existing architecture, clarify consequential uncertainty, and maintain useful project memory under
`docs/` as the project evolves.

## Install

Download or clone this repository. For Codex, copy the repository into one of these locations:

- `.agents/skills/maintain-project-knowledge/` in a project for a project-specific installation.
- `~/.codex/skills/maintain-project-knowledge/` for a personal installation using the default Codex home.

If you configured a different Codex home, use its `skills/` directory instead. The installed folder must contain
`SKILL.md`, `agents/`, and `scripts/`. The README and license can stay with the package. Keep only one version of the
skill in each installation location, and start a new agent turn after installation.

## Use

Include the skill in a normal task:

```text
Use $maintain-project-knowledge while adding CSV export to this project.
```

Or request a documentation review directly:

```text
Use $maintain-project-knowledge to check our project memory against the current implementation.
```

The skill is also discoverable for relevant work. Its [instructions](SKILL.md) define when it applies and the complete
workflow. It asks about unresolved consequential choices while leaving routine implementation decisions to the agent.

## Project memory

The skill maintains English Markdown under `docs/`:

```text
docs/
├── README.md       # Index of core documents
├── architecture.md # Example core document; topics follow the project
├── references/     # Last-resort specialist detail, linked with read conditions
└── evidence/       # Optional raw artifacts linked by their owning document
```

Core memory is read completely in a fresh context and capped at 6,000 words. Claims have one owner and are checked against
active implementation, schemas, configuration, and tests. The agent records durable behavior, boundaries, decisions, and
costly traps; unchanged knowledge needs no documentation edit. The root README stays human-facing and is reviewed when
public onboarding claims are affected.

## Requirements and conventions

The workflow targets Linux and assumes Git, standard shell tools, and Node.js. The validator uses built-in Node modules;
there are no package dependencies. Node.js 22 is used in CI.

The skill includes commit conventions: unless the task or repository says otherwise, completed verified changes are
committed with concise semantic area prefixes. Review [SKILL.md](SKILL.md) before adopting it if your workflow differs.
Documentation maintenance alone does not authorize unrelated code or configuration changes.

From a host repository root, validate its documentation using the installed script:

```sh
node .agents/skills/maintain-project-knowledge/scripts/validate-docs.mjs
```

For a personal installation, use the script's path in that installation instead. The validator checks layout, links,
index coverage, the word limit, and reference/evidence ownership; agents must still verify factual claims.

To check changes to this skill, run from its repository root:

```sh
node --test scripts/validate-docs.test.mjs
```

## License

[MIT](LICENSE), copyright mzmrk.
