---
name: c4
description: Create, update, check and preview C4 architecture models using Structurizr DSL and the Miyorgi C4 binary.
---

# Miyorgi C4

Use this skill to model an architecture from evidence, then let the Miyorgi C4 binary turn that model into a diagram. Keep the Structurizr DSL workspace as the source of truth. The skill helps with meaning. The binary owns geometry.

The skill is architecture-neutral. Do not prescribe tiers, services, stores, event buses, or communication patterns. Model what the user describes. Distinguish confirmed facts, assumptions, and proposals.

## Model from evidence

Follow this workflow whenever you create or update a model.

1. Identify the question the requested diagram must answer. Examples include system context, container responsibilities, a component interaction, an ordered scenario, or a deployment environment. Choose only the view or views that answer that question.
2. Extract the confirmed facts: roles, responsibilities, ownership, interactions, technologies, and deployment facts. Mark a material unknown instead of guessing. Keep a proposed architecture change separate from the current architecture.
3. Classify each evidenced role using C4 meaning. A Person is a human role. A software system is a cohesive capability boundary. A Container is a running application or data store within that system. A Component is an internal responsibility or interface inside one container. Read [modeling.md](references/modeling.md) before changing structure, ownership, shapes, or views.
4. Add or amend the smallest truthful DSL change. Preserve existing aliases, declaration order, relationship direction, unknown properties, and facts outside the request. Do not rewrite a workspace merely to make it look more uniform.
5. Write a full source-grounded description for each changed element. Add `miyorgi.displayDescription` only when the visible responsibility needs a shorter caption. Write relationship labels as the directed action. Read [writing.md](references/writing.md) before changing descriptions or labels.
6. Run `check`, then preview the requested view. Fix model inaccuracies in the DSL. Report a layout limitation without adding coordinates or changing architecture facts to make the diagram fit.

Read [views.md](references/views.md) for component, dynamic, sequence, deployment, and image-view rules. Syntax examples illustrate notation. They do not prescribe an architecture to copy.

## Model for meaning, preview for composition

The layout engine uses truthful model facts to arrange a diagram. Relationship direction expresses who initiates or depends on an interaction. Ownership creates boundaries. View scope determines which elements belong together. These facts help layout, but they must remain true even when the resulting composition is awkward.

Never reverse an arrow, move an element across an ownership boundary, invent a relationship, remove a necessary qualifier, or add coordinates to obtain a preferred position. Do not create a view solely to avoid an engine defect.

After previewing, classify a problem before changing anything:

- If the wrong elements, boundary, relationship, description, or view scope appears, correct the DSL model.
- If the model is truthful but placement, spacing, routing, labels, or text treatment is poor, report a renderer issue or feature request against `c4` with the model, view key, screenshot, expected reading, and actual problem.

Use an explicit `"miyorgi.layoutRole" "actor"` only when an external software system genuinely acts as an entry-point actor. Do not use it to force prominence. See [modeling.md](references/modeling.md#human-and-software-actors).

## Check and preview

```sh
miyorgi c4 check --file workspace.dsl --json
miyorgi c4 serve --file workspace.dsl --view Containers
```

Run `check` after editing. It is read-only and does not run TALA. Resolve syntax or model errors before previewing; review writing warnings against the evidence. A warning is a prompt for judgment, not permission to change meaning. Do not add words or guess technology merely to silence a warning. The writing reference explains the checks and their limits.

Open the local URL printed by `serve`, using the actual requested view key. Verify ownership boundaries, arrow directions, complete captions, shape meanings and readable text. Select an element to inspect its full description. Readable mode preserves the reading floor and may require scrolling; Overview deliberately fits the whole diagram at a smaller scale. Fix model inaccuracies in the DSL; report layout limitations without introducing example-specific coordinates.

The renderer owns a neutral depth palette: Person, Context/System, Container and Component become progressively darker. This is a fixed presentation convention, not model metadata and not an invitation to add arbitrary colour styles. In a static rendering, display descriptions remain visible. In an interactive rendering, the same scene geometry is retained while display descriptions are hidden until hover or keyboard focus; element names and metadata remain centered. Interactive drill-down follows only declared model views, never inferred element names or a fabricated C4 level.

Current views: system landscape, system context, containers, components, dynamic collaboration, dynamic sequence, deployment and embedded image diagrams. Sequence views retain full participant shapes. For the initial release, code diagrams follow Structurizr's approach: retain an externally produced UML/PlantUML diagram as an image view rather than inventing a native code-view model. Editing architecture in the viewer is unsupported. This is a deliberate Structurizr DSL subset; unsupported statements fail. Use the supported neutral depth palette rather than assuming arbitrary themes or style properties work.
