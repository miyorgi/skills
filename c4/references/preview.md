# Preview and presentation

Use the actual URL printed by `miyorgi c4 serve`; its fragment contains the access token. Inspect the requested declared view rather than assuming the first view is the right one. The viewer reads the DSL; architecture editing happens in that file.

Normal reading size preserves legibility, shrinking only as far as 90% when the whole diagram fits. Larger diagrams may need scrolling or explicit zooming. Fit intentionally displays the whole diagram at a smaller scale. These controls require a viewer version that supports them.

Select an element to inspect its full description. Static rendering shows display descriptions. Interactive rendering retains the same geometry but hides those descriptions until hover or keyboard focus; names and metadata remain centered. Drill-down follows declared model views, never inferred element names or fabricated C4 levels. Sequence participants retain their full shapes.

All C4 levels use white interiors and brand-coloured outlines and text without depth shading. Colours darken as needed for contrast; neutral grey is the fallback. See [modeling.md](modeling.md#brand-colours) for ownership and contrast settings.

Classify a visible issue before editing: incorrect elements, ownership, directions, descriptions or scope require a model correction. Poor spacing, routing or text treatment in an otherwise truthful model requires a renderer fix. Capture the model, view key, screenshot, expected reading and actual problem. Do not introduce coordinates or alter architecture facts to disguise a renderer defect.
