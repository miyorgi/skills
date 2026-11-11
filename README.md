# Miyorgi skills

This repository contains skills that help an AI assistant work with Miyorgi products.

## C4

[`c4`](c4/SKILL.md) helps an AI assistant create and update C4 architecture models in Structurizr DSL. It is model-first guidance. It helps the assistant represent architecture truthfully before the Miyorgi C4 binary arranges and renders a diagram.

Use the `c4` directory as a skill package in an agent environment that supports Markdown skills. Use the `miyorgi` skill when you want the product-level entry point.

The skill tells the assistant to:

1. Identify the question that a requested diagram must answer.
2. Extract confirmed facts, assumptions, and proposed changes.
3. Classify roles as C4 people, software systems, containers, or components.
4. Make the smallest truthful change to `workspace.dsl`.
5. Write full descriptions, concise display descriptions, and directed relationship labels.
6. Run the Miyorgi C4 checker and preview the requested view.

The skill does not choose an architecture pattern, save coordinates, reverse relationships to improve a diagram, or invent facts to satisfy a layout rule.

The references explain the detailed rules:

- [`modeling.md`](c4/references/modeling.md) covers C4 classification, ownership, evidence, supported DSL, shapes, and AI actors.
- [`writing.md`](c4/references/writing.md) covers descriptions, display captions, relationship labels, and checker warnings.
- [`views.md`](c4/references/views.md) covers view scope and supported view types.

## Relationship to Miyorgi C4

- The Miyorgi website publishes product documentation, supported releases, and feedback guidance.
- The private C4 implementation repository contains the deterministic Go binary, layout engine, embedded viewer, and review tooling.

The skill guides model authoring. The binary checks the model and turns it into a diagram. The DSL workspace remains the source of truth.

## Start with the native tool

The native tool is currently available for macOS on Apple silicon and Intel.
Install it with Homebrew:

```sh
brew install --cask miyorgi/tap/c4
```

Then run setup from your project folder:

```sh
miyorgi c4 setup
```

The binary includes the C4 skill and installs it for Codex or Claude Code without
Node. Setup records the executable path for the assistant. See
[tool setup](c4/references/tool-setup.md) for setup options or if you already
installed the skill.

## Install the skill first through skills.sh

When this repository is public at `github.com/miyorgi/skills`, users can install
the public skills through skills.sh:

```sh
npx skills add miyorgi/skills
```

skills.sh reports anonymous aggregate install counts. Treat that number as an
adoption signal, not a count of active users.

## Status

Miyorgi C4 is under active development. Treat the skill, supported DSL subset, and viewer interactions as versioned product behaviour. Check the linked C4 repository before relying on an experimental capability.

The skill checks for the native tool before validation or preview. If it is
missing, the skill gives Homebrew installation instructions for macOS. The normal
model source is a single workspace.dsl file; a workspace.json sidecar is not required.
