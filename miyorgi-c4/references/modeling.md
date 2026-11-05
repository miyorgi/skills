# Supported model and stable edits

## Translate architecture into C4

Interpret the architecture rather than transcribing every noun as a box. Identify the scope and viewpoint requested, then map the evidenced roles and responsibilities to C4 kinds:

| Kind | Meaning | Distinction to preserve |
| --- | --- | --- |
| Person | A human role interacting with the architecture | A job title or user role, not an application or a named individual by default |
| Software system | A cohesive set of capabilities providing value to people or other systems | A meaningful system boundary, not automatically every process, vendor or code repository |
| Container | An independently running application or data store within a software system | A C4 container is not specifically a Docker container; a library, class or module is not automatically a container |
| Component | A cohesive responsibility or interface inside a container | An internal unit, not an independently deployed service merely because it is called a component |

Use described responsibilities and operational boundaries to make this classification. If a term such as “service” could mean a system, running application or internal module, resolve it from the evidence; ask when the distinction materially changes the model and remains unknown. Do not manufacture a boundary to avoid the question.

A managed database can belong inside the modeled system even when a cloud vendor operates its infrastructure. A vendor name alone does not make a resource an external software system. Conversely, a separately owned system does not become an internal container merely because it integrates closely. Distinguish modeled system responsibility from hosting location and vendor ownership.

Do not impose a UI/API/store arrangement, microservices, an event bus or any other solution pattern. A valid architecture may have one application, several applications, stores, external services or a different evidenced structure. Model what was described. Recommend changes only when the user asks for architecture advice, and identify them as proposals rather than current facts.

## Evidence and ownership

Use user statements, existing model facts and inspected source/configuration to establish elements, ownership and interactions. Keep assumptions and proposed architecture changes separate from confirmed facts; ask only when a missing fact blocks a correct model change. Do not encode an assumption as an established relationship just to complete a diagram.

A container belongs inside its owning software system. An external software system stays outside that boundary even when tightly integrated. A group expresses an evidenced organizational grouping; it is not a substitute for a system's container boundary and does not change C4 kind or ownership. Never infer ownership or stronger relatedness from drawn proximity. Layout and alignment belong to the binary.

Relationships express the intended interaction or dependency. Preserve the actor or caller initiating it even when data returns in the opposite direction. Do not infer physical data flow, protocol or a reverse interaction from an arrow alone. Missing facts warrant a question when they materially affect kind, ownership, scope or interaction; otherwise retain the known facts and state the uncertainty without inventing detail.

## Human and software actors

An external AI assistant that acts on an author's behalf can remain a software system while taking an actor position in the diagram. When the described architecture establishes this role, add `"miyorgi.layoutRole" "actor"` to that software system's properties. People already receive actor layout. The property changes placement, not kind, ownership or shape; an AI service used as an ordinary dependency should not receive it merely because it uses AI. A `Robot` shape alone does not establish the role.

Preserve who initiates the workflow. For human → assistant → tools, the human leads and the assistant follows. For an autonomous assistant initiating work, the assistant can lead. Retain actual relationship directions and dynamic response markers so return messages do not reverse this order. Do not add relationships or turn software into a Person to force prominence. The layout engine chooses positions from these facts.

## Represent known dependencies at the requested level

The binary does not automatically lift container relationships to their owning software system. A requested system context view can therefore omit a known external dependency if the model only contains the container-level edge. When this happens, represent that same evidenced interaction at system level as well: replace the internal container endpoint with its owning software system, keeping the external endpoint and direction unchanged. Retain the original container relationship for container views.

For example, if an existing relation says an internal application sends invoices to an external accounting system, the owning software system can also have a `Sends invoices to` relationship to that same external system. Ground the label in the existing interaction's purpose. Add a DSL comment immediately above the derived relationship identifying its original aliases, for example `// Context projection of billingApp -> accounting.` This is another view of an existing dependency, not evidence for a new architectural interaction.

Check for an equivalent system-level relationship before adding one. Do not move external systems inside the boundary, invent user-to-container access edges without a known entry point, or infer other relationships to make the views look complete. If several child interactions have different purposes or protocols, preserve distinct grounded relationships or omit technology on a coarser summary; do not invent a single common protocol. Add projections only for views the user needs, and preserve any meaning that matters when summarizing.

For updates, retain aliases, view keys and declaration/relationship order; the binary derives relationship IDs from their order. Preserve untouched names, descriptions, tags, technologies, groups, styles and unknown properties. Add or amend only what the request and evidence justify. Avoid wholesale regeneration of an existing model.

## DSL subset

Use one declaration per line and braces on their own closing lines. The compact forms below describe syntax; expand their blocks across lines in a workspace. They do not prescribe which elements or views a project needs. Supported constructs are:

- `workspace "Name" { model { ... } views { ... } }`.
- `alias = person "Name" "Description"` and `alias = softwareSystem "Name" "Description"`.
- `alias = container "Name" "Description" "Technology"` nested inside its software system; technology may be omitted when unknown.
- `alias = component "Name" "Description" "Technology"` nested inside its container. Component, dynamic, sequence, deployment and image view syntax is in [views.md](views.md).
- `group "Name" { ... }` for a single grouping level; nested groups are unsupported.
- Element blocks with `tags "Tag1,Tag2"` and `properties { "key" "value" }`, with each property on its own line. Unknown element properties are preserved; duplicate keys fail.
- `source -> target "Action label" "Technology"`; declare both endpoints first. Technology is optional. Relationship property/style blocks are unsupported.
- `systemLandscape "Landscape"`, `systemContext systemAlias "Context"`, `container systemAlias "Containers"` and `component containerAlias "Components"`, each with an `include *` block. Use distinct stable view keys. Selective inclusion/exclusion, `autoLayout` and coordinates are unsupported.
- `views { styles { element "Tag" { shape ShapeName } } }`, expanded across lines.

Do not silently translate unsupported constructs into misleading alternatives. A component-shaped container remains a container; declare an actual component inside a container when the evidence establishes that abstraction. State a capability gap if the task needs an unsupported statement.

## Shape mapping

Apply shapes through tags and element shape styles. Match inspected architecture facts or explicit source notation, such as `ContainerDb`, to a shape; do not guess from a name alone.

| Evidence | Shape |
| --- | --- |
| Person | `Person` |
| Database or explicitly modeled database notation | `Cylinder` |
| Object-storage bucket | `Bucket` |
| Filesystem directory | `Folder` |
| Application running in a browser | `WebBrowser` |
| AI assistant represented as a software actor | `Robot` when appropriate to the user's notation |
| No specific presentation evidence | Default rounded box; preserve any explicit shape |

Supported names: `Box`, `RoundedBox`, `Circle`, `Ellipse`, `Hexagon`, `Diamond`, `Person`, `Robot`, `Cylinder`, `Bucket`, `Pipe`, `Folder`, `Component`, `WebBrowser`, `Window`, `Terminal`, `Shell`, `MobileDevicePortrait`, `MobileDeviceLandscape`. Honor explicit user choices among these shapes. Shapes change presentation, not C4 kinds or inferred responsibilities. Arbitrary colors, themes and other Structurizr style properties are not supported.
