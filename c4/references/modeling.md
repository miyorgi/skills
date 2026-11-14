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

Write each evidenced relationship at the most specific level you know. For example, model the application sending invoices to the accounting provider. When implied relationships are enabled, Miyorgi also shows the owning system sending invoices in Context and Landscape views. An explicit relationship takes precedence for the same directed pair. Add a broader explicit relationship only when its purpose needs a distinct summary or the workspace disables inference with `!impliedRelationships false`.

A system-level dependency does not establish which container or component implements it. Use the evidence to identify the entry point or caller. If the installed binary predates inference, check the broader view before adding a grounded summary or suggest an upgrade.

For updates, retain aliases, view keys and declaration/relationship order; the binary derives relationship IDs from their order. Preserve untouched names, descriptions, tags, technologies, groups, styles and unknown properties. Add or amend only what the request and evidence justify. Avoid wholesale regeneration of an existing model.

## DSL subset

Use one declaration per line and braces on their own closing lines. The compact forms below describe syntax; expand their blocks across lines in a workspace. They do not prescribe which elements or views a project needs. Supported constructs are:

- `workspace "Name" { model { ... } views { ... } }`.
- `alias = person "Name" "Description"` and `alias = softwareSystem "Name" "Description"`.
- `alias = container "Name" "Description" "Technology"` nested inside its software system; technology may be omitted when unknown.
- `alias = component "Name" "Description" "Technology"` nested inside its container. Component, dynamic, sequence, deployment, native code and image view syntax is in [views.md](views.md).
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

## Model files and saved layout

Use the DSL workspace as the normal editable model file. Do not generate a workspace.json companion merely for checking, rendering or brand settings. Scene data produced in memory is an implementation detail, not an extra model for the user to maintain.

A user migrating from Structurizr may explicitly want to retain manually adjusted positions or routes stored in a JSON workspace. Preserve that original file. The current Miyorgi CLI does not import saved manual layouts; state the limitation instead of claiming an automatic render preserves those adjustments.

## Brand colours

The source is `workspace.dsl`. Use a `properties` block on the workspace for a
company default, or on a software system, container, component or person for an
override:

```structurizr
properties {
    "miyorgi.brandColor" "#2463EB"
}
```

Use a six-digit hex value. An explicit user preference, including a supplied
palette or neutral styling, takes priority. Use the palette's designated primary
colour when one is given. Preserve colours already set in the DSL unless the
user asks to update them.

When adding or identifying a real company, product or service, automatically
look for its primary brand colour. Confirm the identity from the model and
conversation, then prefer the owner's brand guidelines or official assets when
lookup is available. Use a confidently known primary colour if lookup is
unavailable. Otherwise use neutral grey and continue. A generic provider,
fictional name, ambiguous brand or technology choice alone does not establish
a brand. For example, hosting a bank's API on AWS leaves the API in the bank's
palette. Ask about a palette only when the user wants a specific treatment and
the choice matters.

Apply the colour at the narrowest owning boundary it represents. Use a workspace
default for a workspace dedicated to that brand. Set a separately branded system
or product's colour on its own element, letting its descendants inherit it.
Keep enterprise-wide defaults neutral when several organisations share the model.

The nearest owning element's brand overrides the workspace default. Mark a
separately owned third-party software system with `tags "External"`. Use this tag
for ownership. A bank-owned system still belongs to the bank when shown outside
another system's diagram boundary.

An `External` system and its descendants use neutral grey unless a brand colour
is set on that system or a descendant. Set a provider's brand on its own
software system once its brand colour is supplied or established. Preserve the
`External` tag when updating the provider. Logical instances keep the colour of the system or
container they instantiate in deployment views. Older binaries, including
0.1.0-beta.2, may still inherit workspace colours for `External`; verify the
actual preview before claiming this neutral treatment is available.
Actors using more than one distinct owning software system stay grey, even if
the systems share the same brand or the workspace has a default brand. Multiple
connections to containers of one system count as one system. For one system,
an actor can inherit its brand. Set the actor's own brand property for an
explicit override. This applies to people and software systems marked with
`miyorgi.layoutRole` = `actor`. The rule uses the complete model so changing
views does not recolour actors.

All C4 levels use one brand scheme, with white interiors and no depth shading.
The supplied base colour is used for outlines and text when legible against
white. The renderer darkens it only as needed: at least 3:1 for outlines and
4.5:1 for text. Thus pale brands may need darker text than outlines. Expanded
boundaries use the same white interior and brand outline/text. Shared-brand
groups use that brand; mixed-brand groups remain neutral. People retain the
multi-system neutral rule above. Without a brand, use neutral grey outlines
and dark text. External image diagrams retain their original artwork.

Relationships use their source element's outline colour for the line and
arrowhead, and the source text colour for the action and technology labels.
Return messages follow their actual source. Colour does not change routing.

For an explicitly requested exact-brand comparison, set
`"miyorgi.brandContrast" "none"` in workspace or element properties. Outlines
and text then use the exact colour with no adjustment; source relationships
inherit it too. `"auto"` is the default contrast-aware mode. Do not disable
contrast adjustment unless the user asks.

For a size-aware brand treatment, use `"miyorgi.brandContrast" "readable"`.
Outlines retain the exact brand. Larger bold titles retain it where possible,
darkening only to reach 3:1 on white. Smaller text keeps the brand hue but is
darkened to reach 4.5:1. Relationships use source-coloured strokes with a 3:1
minimum and labels in the source's smaller-text colour. There is no C4-level
shading. Geometry and typography are unchanged.
