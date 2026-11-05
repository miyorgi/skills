---
name: miyorgi
description: Route Miyorgi product work to the appropriate product skill and preserve the product's model-first boundary.
---

# Miyorgi

Use this skill as the entry point for Miyorgi product work. Identify which Miyorgi product the user is working with, then read and follow that product's skill. Do not duplicate product-specific modelling, DSL, or rendering rules here.

## Available product skills

Miyorgi currently has one product skill:

- [C4](../c4/SKILL.md): create, update, check, and preview C4 architecture models.

For a C4 request, read the C4 skill before making a model change. The C4 skill owns C4 classification, architecture evidence, descriptions, views, and model-versus-layout feedback.

## Product boundary

Miyorgi products keep their editable model as the source of truth. An AI assistant helps the user express and update model facts. A deterministic product binary checks, arranges, and renders the model. Do not use this routing skill to infer architecture facts, add coordinates, or prescribe a solution pattern.

This skill is intentionally thin. Add a new product skill only when Miyorgi has a distinct product with its own model, supported operations, and deterministic runtime.
