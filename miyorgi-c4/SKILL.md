---
name: miyorgi-c4
description: Create, update, check and preview C4 architecture models using Structurizr DSL and the Miyorgi C4 binary.
---

# Miyorgi C4

Keep the Structurizr DSL workspace as the source of truth. Translate the described architecture into accurate C4 abstractions; distinguish confirmed facts, assumptions and proposed changes. The skill is architecture-neutral: do not prescribe tiers, services, stores or communication patterns. The binary owns layout. Do not add coordinates, change ownership or shorten facts to make a diagram fit.

## Author or update

Read [modeling.md](references/modeling.md) when creating or changing model structure, views or shapes. Read [writing.md](references/writing.md) when creating or changing descriptions or relationship labels. These references define the supported DSL and caption conventions.

Preserve existing aliases, declaration order, relationship direction, unknown properties and facts outside the requested change. Prefer a small deterministic edit over rewriting the workspace. Keep full element descriptions; use `miyorgi.displayDescription` for a separate concise caption where needed. Write responsibilities and interactions as specific actions rather than lists of nouns. Do not invent a responsibility to make the wording more specific.

Choose the C4 kinds and boundaries from the architecture's purpose, responsibilities and ownership, using the modeling reference. Reuse the user's domain vocabulary and existing aliases. Create only the views needed for the request; supported view types are capabilities, not a required set. Read [views.md](references/views.md) for component, dynamic, sequence, deployment, native code or image views. Syntax examples illustrate notation, not an architecture to copy.

## Check and preview

```sh
miyorgi-c4 check --file workspace.dsl --json
miyorgi-c4 serve --file workspace.dsl --view Containers
```

Run `check` after editing. It is read-only and does not run TALA. Resolve syntax or model errors before previewing; review writing warnings against the evidence. A warning is a prompt for judgment, not permission to change meaning. Do not add words or guess technology merely to silence a warning. The writing reference explains the checks and their limits.

Open the local URL printed by `serve`, using the actual requested view key. Verify ownership boundaries, arrow directions, complete captions, shape meanings and readable text. Select an element to inspect its full description. Readable mode preserves the reading floor and may require scrolling; Overview deliberately fits the whole diagram at a smaller scale. Fix model inaccuracies in the DSL; report layout limitations without introducing example-specific coordinates.

The renderer owns a neutral depth palette: Person, Context/System, Container and Component become progressively darker. This is a fixed presentation convention, not model metadata and not an invitation to add arbitrary colour styles. In a static rendering, display descriptions remain visible. In an interactive rendering, the same scene geometry is retained while display descriptions are hidden until hover or keyboard focus; element names and metadata remain centered. Interactive drill-down follows only declared model views, never inferred element names or a fabricated C4 level.

Current views: system landscape, system context, containers, components, dynamic collaboration, dynamic sequence, deployment and embedded image diagrams. Sequence views retain full participant shapes. For the initial release, code diagrams follow Structurizr's approach: retain an externally produced UML/PlantUML diagram as an image view rather than inventing a native code-view model. Editing architecture in the viewer is unsupported. This is a deliberate Structurizr DSL subset; unsupported statements fail. Use the supported neutral depth palette rather than assuming arbitrary themes or style properties work.
