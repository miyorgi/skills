# Additional diagram views

Choose a view for the question being answered. More detail is not automatically
more useful. Reuse one logical model across views; only deployment instances add
physical identities. Do not invent components or infrastructure to populate a view.

## Components

A component is a cohesive internal responsibility inside a container. It does not
run independently merely because the model shows it separately. Declare it within
the owning container, with the same description and technology conventions:

```text
statements = component "Statement service" "Builds monthly account statements." "Java"
```

In `views`, scope the view to that container:

```text
component backend "Components" {
    include *
}
```

The view shows the container's components and directly related people, systems or
containers. External containers remain outside the focused container boundary.
Do not model every class as a component or promote components to containers merely
to make them visible.

## Dynamic collaboration and sequence

Describe a specific scenario as ordered occurrences of existing relationships.
Use scenario-specific action labels without changing the logical dependency. A
dynamic scope can be a software system, container, or `*` for system-level traffic.
At container scope, internal participants are its components; other containers,
people and systems can participate at the boundary.

```text
dynamic backend "SignIn" {
    ui -> signInController "Submits credentials"
    signInController -> security "Checks credentials"
    security -> signInController "Returns verification result"
    signInController -> ui "Returns session"
}
image backend "SignInSequence" {
    plantuml "SignIn"
}
```

This example requires the corresponding forward model relationships to already
exist. Reverse occurrences can represent responses; do not add a reverse logical
dependency just to draw a response. If multiple modeled relationships exist for
the same endpoints, use the exact modeled description to disambiguate. Repeated
messages stay separate and retain declaration order. Labels may use explicit
numeric prefixes such as `1:`; otherwise the binary numbers them in order.

The `plantuml` reference above selects the native sequence presentation of an
existing dynamic view with the same scope; it does not run PlantUML. Participant
shapes keep their full measured dimensions, metadata and descriptions. Alternatively,
set `"miyorgi.presentation" "sequence"` in a dynamic view's `properties` block.

The supported scenario syntax is a serial sequence. Do not claim conditional or
parallel fragments, activation bars, or arbitrary UML semantics that the source
does not express. Interaction order describes a scenario, not necessarily global
concurrency or timing guarantees.

## Deployment

Keep logical ownership separate from physical hosting. A deployment node describes
an execution environment; a container instance references an existing logical
container and retains its shape and description. Reuse one logical container for
multiple evidenced deployments. Do not create a new logical system for each server.

Inside `model`, declare environments and nested nodes:

```text
deploymentEnvironment "Production" {
    host = deploymentNode "Application host" "Runs the statement application." "Linux" {
        appInstance = containerInstance backend
    }
    dataHost = deploymentNode "Database host" "Persists account records." "PostgreSQL" {
        databaseInstance = containerInstance database
    }
}
```

Select that environment in `views`:

```text
deployment banking "Production" "ProductionDeployment" {
    include *
}
```

`softwareSystemInstance systemAlias` represents an instance of an external system.
`infrastructureNode "Name" "Description" "Technology"` represents infrastructure
such as an evidenced load balancer. Deployment nodes can nest and carry `instances
"2"` to express a known replica count. Groups preserve organizational grouping.
Do not infer replicas, failover, hosting vendors or networks from the logical view.

Logical relationships are projected between their instances in the selected
environment. Explicit instance or infrastructure relationships are also supported.
If importing a source with a complete explicit physical topology, set
`"miyorgi.deploymentRelationships" "explicit"` in the deployment view properties
to use only that topology. Do not use this switch to hide unknown dependencies.

## Native Code views

Use native code elements when implementation details are available. They are a
Miyorgi extension, not standard Structurizr DSL. Nest them inside the component
they implement, and preserve evidenced names and signatures:

```text
adapter = component "Persistence adapter" {
    repository = codeElement "OrderRepository" "" {
        properties {
            "miyorgi.codeKind" "interface"
            "miyorgi.namespace" "orders"
            "miyorgi.visibility" "+"
        }
        member "Find(id OrderID) (Order, error)"
    }
    storage = codeElement "PostgresRepository" "" {
        properties {
            "miyorgi.codeKind" "struct"
        }
        member "connection *sql.DB"
    }
    storage -> repository "Implements"
}
```

Inside `views`, declare:

```text
code adapter "Code" {
    include *
}
```

Supported `miyorgi.codeKind` values: `class`, `interface`, `abstract`, `enum`,
`record`, `struct`, `function`, `module`. Omit the property if the kind is unknown.
Visibility uses `+` public, `-` private, `#` protected or `~` package-private when
those meanings apply. Members are nonblank opaque strings: preserve source
signatures rather than translating or inventing them. Namespace is retained in
details. Relationships use ordinary directed arrows and explicit labels such as
"Implements"; full UML arrow notation is not supported. Do not infer implementation
classes from a higher-level architecture description. Missing details are a gap
to report, not a reason to fabricate a Code view.

## Other embedded diagrams

An optional image view scoped to a component can preserve a supplied SVG, PNG or
JPEG using `image adapter "Reference" { image "data:image/svg+xml;base64,..." }`.
The payload must be real base64; the ellipsis illustrates syntax only. The binary
does not execute inline PlantUML or fetch remote images. Image text uses intrinsic
image size rather than native C4 text measurement.

Run `check` for all declared views, then preview the requested views. For sequence,
verify chronology and responses; for deployment, verify environment, nesting,
replicas and instance identity; for code, verify member signatures, ownership and
relationship direction against the implementation evidence.
