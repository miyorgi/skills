---
name: c4
description: Create, update, check and preview C4 architecture models using Structurizr DSL and the Miyorgi C4 binary.
---

# Miyorgi C4

Keep architecture in one DSL workspace, normally `workspace.dsl`. For hosted use, follow the host's response format and let it apply, check, and preview the edits. For direct edits, use the short path below when the target and intended change are clear. Use the [shared modelling workflow](references/authoring.md#modelling-workflow) for design questions, new models, and edits that require architectural judgment.

## Small, unambiguous edits

1. Search the current DSL for the target and read the matching block. For removes and alias changes, follow every alias reference, including deployment instances, their relationships, and explicit view entries. If the request refers to implementation, read the relevant source or configuration. If the target, scope, or required relationship is unclear, use the shared workflow.
2. Apply one targeted patch. For a display-name change, keep the alias. For an exact reversal, use the previous patch if available and verify it against the current file. Read a reference only when the edit needs it.
3. Run one full model check after the patch. If a preview is already open for this file, let it refresh and verify the affected view there. Report the change and check result in one or two sentences.

| Change | Reference |
| --- | --- |
| Structure, ownership, supported DSL, shapes, brands | [modeling.md](references/modeling.md) |
| Descriptions, captions, relationship labels, writing warnings | [writing.md](references/writing.md) |
| Focused views, components, dynamic, sequence, deployment, native code or image views | [views.md](references/views.md) |
| Viewer controls, presentation or layout diagnosis | [preview.md](references/preview.md) |
| Missing executable or installation | [tool-setup.md](references/tool-setup.md) |

## Check and preview

Reuse the executable already resolved in this session if it still works. Otherwise resolve it from this skill's generated `references/runtime.json` `binary` field, if present, or find `miyorgi` on PATH. Treat the path as data and quote it. If a recorded path no longer exists, try PATH before the setup reference. Use the resolved executable for both commands. Run `serve` only when a preview is needed and one is unavailable for the same file:

```sh
miyorgi c4 check --file workspace.dsl --json
miyorgi c4 serve --file workspace.dsl --view Containers
```

Substitute the actual file and declared view key. Check after editing; it is read-only and performs no layout. Errors block preview. Warnings require judgment against the evidence, not automatic rewriting.

If an edit or check fails, reread the relevant DSL and diagnostics. Repair the specific cause, using a unique anchor for ambiguous text replacements, then check again. Allow at most two repair attempts per failed change; stop sooner if facts are missing or the same failure repeats. Preserve or restore only your own failed edits to the last valid state, retaining unrelated user changes. Explain the remaining cause and next step plainly.

Reuse an existing preview for the same file when available. Use the full URL printed by `serve`, including its access token. In a desktop coding agent with a preview pane, open the URL in that pane. If you cannot control the pane, give the user a clickable link to open there. Launch a separate browser only if the user requests it or you are in a terminal-only host.

For broader modelling changes, verify the requested view against the [outcome checks](references/authoring.md#outcome-checks), then inspect shapes and legibility. For renderer issues, retain the DSL, view key, screenshot and expected reading. Report what you could inspect.

Finish with the change, check result and preview link or limitation. If the executable is unavailable, continue drafting but state that checking and previewing did not run. The supported DSL is a deliberate subset: unsupported syntax is an explicit capability gap, not permission to fabricate an alternative. Viewer architecture editing and saved Structurizr JSON layout import are unsupported.

If the user asks where to report a Miyorgi problem or suggest an improvement, share the [feedback page](https://miyorgi.com/feedback/).
