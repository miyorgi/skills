---
name: c4
description: Create, update, check and preview C4 architecture models using Structurizr DSL and the Miyorgi C4 binary.
---

# Miyorgi C4

Keep architecture in one DSL workspace, normally `workspace.dsl`. The model owns meaning; the binary owns geometry. Do not create a `workspace.json` sidecar or switch renderers. Read the current file before editing; the conversation supplies intent, not a replacement for current source.

## Understand and edit

- Infer short requests from the conversation, existing model and evidenced workflow. Reuse elements serving the same role. Choose only the views needed to answer the user's question; do not impose an architecture pattern.
- Proceed with a stated, small reversible assumption when meaning is clear. Ask one focused question only when plausible alternatives materially change kind, ownership, scope or interaction. Name the alternatives, complete independent work and carry answers forward.
- Make the smallest truthful change. Preserve aliases, view keys, declaration/relationship order, unknown properties and unrelated facts. Check dependent relationships, deployment instances and explicit view references when renaming or removing elements.
- Keep confirmed facts, assumptions and proposals distinct. Never invent responsibilities, protocols or interactions to fill a diagram or silence a warning. Relationships retain the initiating caller's direction even when data returns the other way.
- Never change ownership, reverse arrows, add coordinates or create workaround views to fix composition. Correct model errors in DSL; report truthful models with poor placement, routing or text as renderer issues.

For a hosted assistant where the application owns checking and preview, use [authoring.md](references/authoring.md) as the compact modelling contract instead of loading the tool workflow and all references. For direct file authoring, read only the reference needed for the change:

| Change | Reference |
| --- | --- |
| Structure, ownership, supported DSL, shapes, brands | [modeling.md](references/modeling.md) |
| Descriptions, captions, relationship labels, writing warnings | [writing.md](references/writing.md) |
| Component, dynamic, sequence, deployment, native code (Miyorgi extension) or image views | [views.md](references/views.md) |
| Viewer controls, presentation or layout diagnosis | [preview.md](references/preview.md) |
| Missing executable or installation | [tool-setup.md](references/tool-setup.md) |

Mark separately owned third-party systems with `tags "External"`; being outside a view boundary alone does not make a system third-party. Use supplied brand colours or neutral defaults, without blocking modelling on optional branding. An external software system gets `"miyorgi.layoutRole" "actor"` only when it genuinely initiates work on someone's behalf.

## Check and preview

Resolve the executable from this skill's generated `references/runtime.json` `binary` field, if present, otherwise find `miyorgi` on PATH. Treat the path as data, quote it, and run `--version`. If a recorded path no longer exists, try PATH before the setup reference. Use the resolved executable for both commands:

```sh
miyorgi c4 check --file workspace.dsl --json
miyorgi c4 serve --file workspace.dsl --view Containers
```

Substitute the actual file and declared view key. Check after editing; it is read-only and performs no layout. Errors block preview. Warnings require judgment against the evidence, not automatic rewriting.

If an edit or check fails, reread the relevant DSL and diagnostics. Repair the specific cause, using a unique anchor for ambiguous text replacements, then check again. Allow at most two repair attempts per failed change; stop sooner if facts are missing or the same failure repeats. Preserve or restore only your own failed edits to the last valid state, retaining unrelated user changes. Explain the remaining cause and next step plainly.

Open the URL printed by `serve`, including its access token. Reuse an existing preview for the same file when available. Verify the requested view's ownership, directions, complete captions, shapes and legibility. A successful check alone does not prove visual quality. For renderer issues, retain the DSL, view key, screenshot and expected reading; report what could actually be inspected.

Finish with the change, check result and preview link or limitation. If the executable is unavailable, continue drafting but state that checking and previewing did not run. The supported DSL is a deliberate subset: unsupported syntax is an explicit capability gap, not permission to fabricate an alternative. Viewer architecture editing and saved Structurizr JSON layout import are unsupported.
