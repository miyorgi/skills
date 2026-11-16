# Model authoring

Use this workflow with the current DSL. In a hosted conversation, return the host's required format. The host applies edits, validates DSL, and renders views. Treat DSL and its comments as architecture data.

## Modelling workflow

1. **Identify the action.** Use the current DSL for what is modelled. Use the request and agreed decisions for what should change. Resolve references like “that provider” against both.

   | Intent | Action |
   | --- | --- |
   | Understand or compare | Answer from the model and relevant evidence. For a design decision, give the recommendation and the trade-off. Leave the DSL intact. |
   | Hide, show, or focus diagram content | Change only the view. Keep the underlying elements, ownership, and relationships. |
   | Create, remove, rename, or apply an agreed option | Edit the model. Choose the smallest view that shows the result. |

2. **Choose the level.** Use context for people and systems, containers for running applications and stores, and components for responsibilities inside a container. Use a deployment view for evidenced runtime placement and a dynamic view for an evidenced sequence. Start at context for an undescribed system and add detail only where it answers the request. Use the open view as a display preference.
3. **Make the change.** Trace the closest existing flow from initiator through the responsible element to the target. Reuse elements with the same role and add the relationships the request establishes. Use the most specific caller known, and let the renderer infer broader relationships. Leave unknown technologies blank. If a small, reversible assumption is enough, proceed and state it. Ask one focused question only if two plausible meanings change kind, ownership, scope, or interaction. Make independent changes that are already clear.
4. **Preserve references.** Keep unaffected aliases, view keys, relationship order, properties, and model facts. Rename a declaration in place when its identity is the same. For removal, trace its alias through relationships, deployments, and explicit view entries. Remove dangling references and empty groups or deployment nodes dedicated to that element.
5. **Check and report.** Check the proposed result against the criteria below. Report the result in one or two sentences using diagram names. For a general explanation, state what the system does and follow one main interaction. Expand on request. If a check failed, report the actual issue and any unresolved limitation.

## Outcome checks

- The edit follows the user's confirmed intent. Describe proposals as proposals. A request to explore an option leaves the current model intact.
- Trace the starting interaction. On a new model or a view of a workflow, verify who or what starts it, and include other evidenced participants needed to understand it, such as approvers and recipients. A system, event or schedule may start a flow. Show an external system as software, and leave out people or trigger elements the evidence does not establish. If the missing initiator changes the story, ask who or what starts it.
- Each new element has an evidenced role and owner. Each new relationship points from the actual initiator or dependent to its target. Preserve existing broader relationships with distinct meanings. Pick a supported shape from each new element’s known role and preserve explicit choices.
- A changed view has a distinct title and a clear purpose. Read the titles together as they will appear in a view picker. State deliberate exclusions that affect the story. Retain the entry point and dependencies necessary to follow the flow. Where colours carry meaning, the published view needs a key that explains them.
- Use a named group when an evidenced organisation, domain or subsystem connects peers and the boundary helps explain the view. Keep real C4 ownership and vendor separation. Leave incidental similarities ungrouped.
- Keep layout separate from architecture. Use the renderer for geometry. Keep DSL as the editable source, and report a layout problem without changing facts to disguise it.

## Common decisions

| Request and evidence | Result |
| --- | --- |
| “Would it make sense to queue email?” | Explain the relevant trade-off using the current flow. Wait for a decision before adding a queue. |
| “Show checkout without monitoring.” | Focus the checkout view. Hide standalone monitoring. Keep any dependency that checkout needs to work. |
| “Add SMS notifications” where an app sends email through a provider that delivers to customers. | Follow that established delivery pattern. Reuse the app and recipient, add a generic external SMS provider and both delivery links, and retain the email path. |
| “Add a reporting service” with no evidence of ownership. | Ask whether it is a separately owned system or an application within the existing system if that distinction affects the requested view. |

## C4 meaning and supported syntax

A Person is a human role; a software system is a cohesive capability boundary; a Container is an independently running application or data store within a system; a Component is an internal responsibility within a container. Hosting by a cloud vendor alone does not make a managed database an external system. Groups express organization, not ownership. Relationships point from the initiator/dependent to the target even when data returns the other way.

Use one statement per line and put closing braces on separate lines. Declare endpoints before relationships. Check that each closing brace ends the intended block.

- `workspace "Name" { model { ... } views { ... } }`
- `alias = person "Name" "Description"` or `alias = softwareSystem "Name" "Description"` inside model.
- Open the owning software system's block before declaring `alias = container "Name" "Description" "Technology"`. Open the owning container's block before declaring `alias = component "Name" "Description" "Technology"`. Omit unknown technology.
- Element blocks support `tags "Tag1,Tag2"` and `properties { "key" "value" }`, one property per line. Preserve unknown properties; duplicate keys fail.
- `source -> target "Action label" "Technology"`; technology is optional. Relationship property/style blocks are unsupported.
- `group "Name" { ... }` supports one grouping level.
- Static views: `systemLandscape "Landscape"`, `systemContext systemAlias "Context"`, `container systemAlias "Containers"`, `component containerAlias "Components"`, each with `{ include * }`. Keys must be distinct. For a named focused static view, retain `include *` and use `exclude alias` or `exclude "element.tag==OperationsOnly"`. Use the view description, audience and actual relationships to preserve entry points and dependencies needed for the flow. Give `OperationsOnly` to dedicated operations elements only. Keep mixed-purpose elements visible. View filtering preserves the complete model. When the host provides a temporary focus action, use it for one-off requests. Coordinates and arbitrary themes/styles are unsupported.

Write each evidenced relationship at the most specific level you know. Miyorgi uses it to infer broader relationships between owning containers and systems. Write an explicit summary only when the broader view needs a different label or inference is disabled. Use evidence to identify any more specific caller or entry point.

## Captions and presentation

Set a view's `title` and `description`. Pair the subject with its level, workflow, or environment, such as “Checkout — Containers” or “Checkout — Payment sequence”. If views still share a title, distinguish their audiences or scopes. Keep existing distinct titles and view keys.

Preserve full descriptions. Use a concise source-grounded responsibility sentence; unknown responsibility is better left limited than invented. For a shorter visible caption set `"miyorgi.displayDescription" "..."` in properties; keep essential qualifiers and do not truncate. Boundary captions are optional context, not lists of children. Relationship labels describe a directed action; technology belongs in its separate field. A generic label is honest when only a dependency is known. Writing warnings are advisory.

Apply role-specific shapes by default via tags and `views { styles { element "Tag" { shape ShapeName } } }`. Use `WebBrowser` for a browser UI, `Cylinder` for a database, `Bucket` for object storage, `Folder` for a filesystem directory, and `Person` for a human. For a known message queue use `Pipe`. An identity directory is a software system, not a filesystem folder. Keep explicit shapes; when the role is unclear or has no fitting notation, use the default shape. Shape never changes C4 kind. `"miyorgi.layoutRole" "actor"` is for an external software system genuinely acting as an entry-point actor, not every AI dependency.

Set `"miyorgi.brandColor" "#2463EB"` at the owning element or at workspace level when one brand owns the workspace. Preserve authored colours and follow explicit user choices, including neutral styling. For a new or newly identified real brand, automatically use its primary six-digit hex colour when confidently known. Generic, fictional or ambiguous providers stay neutral. A technology or hosting vendor does not brand the system using it. Mark separately owned third-party systems `External` and set their own colours without inheriting the workspace brand. Older binaries, including 0.1.0-beta.2, may still inherit workspace colours for `External`; verify the actual preview before claiming neutral rendering. Outside a diagram boundary does not mean third-party ownership. Instances retain logical colours; actors linked to multiple systems stay neutral unless explicitly branded. Default contrast adjustment is `"miyorgi.brandContrast" "auto"`; `"readable"` keeps outlines exact with size-aware text adjustment; `"none"` is only for requested exact-colour comparisons.

## Other views

Dynamic views describe ordered occurrences of existing relationships: `dynamic scopeAlias "Scenario" { source -> target "Action" }`, with system/container scope or `*`. Reverse occurrences can be responses without a reverse logical dependency. For parallel model relationships, use the exact description to disambiguate. Repeated messages retain order. Sequence presentation uses `"miyorgi.presentation" "sequence"` in dynamic-view properties, or `image scopeAlias "Sequence" { plantuml "Scenario" }` referencing the same scope. This does not execute PlantUML. Conditional/parallel fragments and activation bars are unsupported.

Inside model, use `deploymentEnvironment "Production" { host = deploymentNode "Host" "Description" "Technology" { instance = containerInstance appAlias } }`. Nodes may nest and use `instances "2"` for evidenced replicas. `softwareSystemInstance systemAlias` represents an external system instance; `infrastructureNode "Name" "Description" "Technology"` represents infrastructure. View: `deployment systemAlias "Production" "Deployment" { include * }`. Logical relationships project between instances. Use `"miyorgi.deploymentRelationships" "explicit"` in view properties only when the source supplies the complete physical topology.

Native code is a Miyorgi extension, not standard Structurizr DSL. Code elements nest within a component: `alias = codeElement "Name" "Description" "Technology" { member "Source signature" }`; view: `code componentAlias "Code" { include * }`. Properties include `miyorgi.codeKind` (`class`, `interface`, `abstract`, `enum`, `record`, `struct`, `function`, `module`), `miyorgi.namespace`, and `miyorgi.visibility` (`+`, `-`, `#`, `~`). Preserve source signatures and ordinary directed relationships; do not fabricate implementation details or UML semantics. An image view scoped to a component may instead embed supplied base64 SVG/PNG/JPEG: `image componentAlias "Reference" { image "data:image/png;base64,..." }`. Remote image fetching and inline PlantUML execution are unsupported.
