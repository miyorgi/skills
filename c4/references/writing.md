# Architecture descriptions and relationship labels

## Element responsibilities

For new English captions, use a present-tense, third-person verb followed by a specific domain object. Use the element name as the implied subject. Prefer one sentence ending in a full stop, usually 5–14 words; this is guidance, not a limit. Preserve necessary distinctions over brevity. Never truncate text automatically or replace meaningful words with vague shorthand to fit a box.

| Element | Question the caption answers | Grounded example |
| --- | --- | --- |
| Person | What role or action makes this person relevant? | `Places orders and tracks their fulfilment.` |
| Software system | What outcome does it provide, and for whom? | `Lets customers view accounts and make payments.` |
| Container | What responsibility does this application or store own? | `Validates orders and coordinates fulfilment requests.` |
| Data store | What domain information does it retain? | `Stores customer accounts and access logs.` |

These are patterns, not facts to reuse. A person's known role can justify `Supports bank customers.`; a name alone cannot justify capabilities. If the evidence only identifies a role or technology, keep that limited description and note the missing responsibility rather than inventing one. Preserve the user's requested language; these English grammar conventions are not a reason to translate existing content.

Avoid noun lists such as `Online account information and payments.` when the source says the system lets customers view accounts and make payments. Avoid generic claims such as `Handles business logic.` when a supported, specific responsibility is available. Put implementation technology in the technology field, not in place of the responsibility.

## Full description and visible caption

Keep the existing full element description intact unless correcting its facts or explicitly revising it is in scope. When it is too detailed for a caption, add:

```text
properties {
    "miyorgi.displayDescription" "Lets customers view accounts and make payments."
}
```

The full description stays in the DSL and generated scene and is available through viewer selection. The display property controls the visible caption. Ordinary shapes fall back to the full description when it is absent; boundaries show only their name and type. An explicitly blank value is invalid. A short, clear full description needs no duplicate property for an ordinary shape. Preserve other properties.

For a system, container or deployment-node boundary, add a display description only when one short responsibility statement contributes context beyond its name, type and visible contents. Do not repeat the full paragraph, list its children or restate its technology. Preserve the full description for inspection even when the boundary has no visible caption. This is a presentation choice; never change ownership or invent responsibilities to improve the picture.

Derive each caption from that element's full description and explicitly modeled interactions. Keep important qualifiers, such as customer versus staff access, read-only versus write access, or payments versus account reporting. Do not infer an element's responsibility from an incoming arrow alone: a system receiving email is not necessarily an email-delivery system.

## Relationships

Read each relationship as **source → label → target** to check the interaction makes sense. This is a meaning check, not a requirement that concatenating their names forms a literal grammatical sentence. Both `Reads account balances` and `Requests balances from` can be useful labels when supported by the endpoints and evidence. A shape caption describes owned responsibility; an arrow label describes the actual directed interaction.

Use a short present-tense action phrase, normally without a full stop or repeated element names. Prepositions are useful when the target supplies the object.

| Source → target | Label | Separate technology |
| --- | --- | --- |
| Customer → Orders | `Places orders using` | `HTTPS` if known |
| Order API → Order Store | `Stores and retrieves orders in` | `SQL` if known |
| Email Service → Customer | `Sends account notifications to` | Leave blank if unknown |

Use the actual evidence, not this table, to choose verbs and objects. Keep transport, serialization or protocol in the optional technology argument, for example `api -> store "Stores and retrieves orders in" "SQL"`. The renderer formats metadata; do not add brackets to DSL technology strings or insert metadata into the label.

Prefer specific labels over `Uses`, `Calls` or `Connects to` when the interaction is known. If only a dependency is evidenced, a generic label is honest: retain it and flag missing detail rather than guessing. Usually a label is shorter than an element caption; the 5–14-word caption guideline is not a minimum for labels. Preserve essential facts and protocol details rather than enforcing a word count. Relationship labels currently have no separate display-description property, so do not shorten away facts expecting a hidden full version to survive.

Do not infer protocol or reverse an arrow to improve grammar or layout. A request from an application to a database can read `Reads orders from`; its direction still expresses the application's dependency on the database, not the returned data flow. Add a separate reverse relationship only when it represents an evidenced interaction.

## Review writing diagnostics

`miyorgi c4 check --file workspace.dsl --json` validates the model without layout. Its JSON report has `version`, `valid` and `diagnostics`; each diagnostic has `severity`, `code`, `message` and relevant element/relationship/view/field identifiers. Invalid input or model errors exit nonzero; valid models exit zero even with warnings.

Writing warnings are advisory: `caption.missing`, `caption.too_long` (over 14 whitespace-separated words), `caption.multiline`, `caption.vague`, `caption.noun_phrase` and `description.missing`. Vague/noun-phrase checks recognize a small set of common patterns, not arbitrary English grammar. They do not prove factual accuracy, ownership, good writing or visual quality.

Inspect each diagnostic in context. Keep a source-grounded exception when resolving it would require inventing facts, removing a necessary qualifier or rewriting out-of-scope content. Report unresolved substantive uncertainty separately from grammar preferences. Passing the check does not replace reading the model and preview.
