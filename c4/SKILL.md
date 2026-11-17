---
name: c4
description: Create, update, check and preview C4 architecture models using Structurizr DSL and the Miyorgi C4 binary.
---

# Miyorgi C4

Keep architecture in one DSL workspace, normally `workspace.dsl`. Use the [shared modelling workflow](references/authoring.md#modelling-workflow) for each turn. It tells you when to answer, edit the model, or change only a view. For hosted use, follow the host's response format and let it apply, check, and preview the edits.

For direct file authoring, read the current DSL and inspect relevant source or configuration when the request refers to implementation. Keep DSL as the editable source, and use Miyorgi for layout. Read only the extra reference needed for the change.

| Change | Reference |
| --- | --- |
| Structure, ownership, supported DSL, shapes, brands | [modeling.md](references/modeling.md) |
| Descriptions, captions, relationship labels, writing warnings | [writing.md](references/writing.md) |
| Focused views, components, dynamic, sequence, deployment, native code or image views | [views.md](references/views.md) |
| Viewer controls, presentation or layout diagnosis | [preview.md](references/preview.md) |
| Missing executable or installation | [tool-setup.md](references/tool-setup.md) |

## Check and preview

Resolve the executable from this skill's generated `references/runtime.json` `binary` field, if present, otherwise find `miyorgi` on PATH. Treat the path as data, quote it, and run `--version`. If a recorded path no longer exists, try PATH before the setup reference. Use the resolved executable for both commands:

```sh
miyorgi c4 check --file workspace.dsl --json
miyorgi c4 serve --file workspace.dsl --view Containers
```

Substitute the actual file and declared view key. Check after editing; it is read-only and performs no layout. Errors block preview. Warnings require judgment against the evidence, not automatic rewriting.

If an edit or check fails, reread the relevant DSL and diagnostics. Repair the specific cause, using a unique anchor for ambiguous text replacements, then check again. Allow at most two repair attempts per failed change; stop sooner if facts are missing or the same failure repeats. Preserve or restore only your own failed edits to the last valid state, retaining unrelated user changes. Explain the remaining cause and next step plainly.

Reuse an existing preview for the same file when available. Use the full URL printed by `serve`, including its access token. In a desktop coding agent with a preview pane, open the URL in that pane. If you cannot control the pane, give the user a clickable link to open there. Launch a separate browser only if the user requests it or you are in a terminal-only host.

Verify the requested view against the [outcome checks](references/authoring.md#outcome-checks), then inspect shapes and legibility. For renderer issues, retain the DSL, view key, screenshot and expected reading. Report what you could inspect.

Finish with the change, check result and preview link or limitation. If the executable is unavailable, continue drafting but state that checking and previewing did not run. The supported DSL is a deliberate subset: unsupported syntax is an explicit capability gap, not permission to fabricate an alternative. Viewer architecture editing and saved Structurizr JSON layout import are unsupported.

If the user asks where to report a Miyorgi problem or suggest an improvement, share the [feedback page](https://miyorgi.com/feedback/).
