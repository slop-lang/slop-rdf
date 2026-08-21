## General Guidelines

Whenenver you are working with SLOP code, you are permanently in SLOP PAREDIT MODE — structural editing only for the Slop language (https://github.com/slop-lang/slop).

Core rules — break any and the edit is invalid:
1. ALL code output MUST have PERFECTLY balanced parentheses ( and ). Count obsessively.
2. Edit structurally ONLY: describe changes as slurp/barf/splice/raise/wrap/unwrap/kill-sexp/etc BEFORE showing code.
   - Treat @-forms (@intent, @spec, @pre, @post), (hole ...), (fn ...), (type ...), (module ...), (ffi ...), etc. as atomic units when possible.
   - For {infix conditions}, treat the entire {} as one balanced sexp unit — never insert parens inside {} unless explicitly structural.
3. Before ANY code block, insert <slop-paren-audit>:
   - Count every ( and ) across the entire proposed code.
   - Report: "Open parens: X    Close parens: X    Balanced: YES/NO"
   - If NO → do NOT output code; instead say "Unbalanced — retrying structurally" and fix in thinking.
4. When editing:
   - Show → BEFORE: the targeted form(s), marked →( like this )←
   - Describe: exact structural ops (e.g. "wrapping the body in (if ...)", "slurping next arg into fn params", "raising @pre one level", "inserting typed hole at end of body")
   - Then → AFTER: full balanced form/module snippet
5. If user request risks breaking Slop structure (e.g. raw text edits to infix {}, unbalanced ranges, missing @spec), refuse and suggest structural equivalent.
6. Favor Slop idioms: keep @-contracts together at top of fn, use range types, prefer holes for implementation, indent generously, align bindings/conditions.
7. This is Slop — symbolic, contract-first, LLM-optimized. No textual slop. Output clean, balanced s-expressions only.



<!-- moosedev:begin — MOOSEDev project-memory workflow. Managed by `moosedev init`; edit around this block freely, or delete the whole begin…end block to opt out. -->
> This project uses the **MOOSEDev** MCP server for durable, structured, long-term memory.
> The typed **project knowledge graph is the source of truth** for architectural decisions,
> lessons, constraints, requirements, and patterns — **not** markdown files. Free-text notes
> (e.g. `tasks/lessons.md`, `tasks/todo.md`) are optional human-readable mirrors, never canonical.

## Working with project memory (MOOSEDev)

When the `moosedev` MCP tools are available, prefer them over re-deriving context from scratch.
The loop:

1. **Recall first.** Before non-trivial work, surface prior decisions/lessons/constraints from
   the graph — and show the queries you ran. "Recall first" means a **list-all**
   `get_relevant_context` (no `topic`), not only a topic probe: a topic-scoped empty result
   means nothing cleared the relevance floor, **not** that the graph is empty.
2. **Capture as typed records.** Record durable knowledge as you go with
   `record_important_decision` (pick the right `kind`). Capture the decision **and its
   rationale** (and the rejected alternative), not transient chatter. Always report what was
   written (kind / title / returned IRI) — no silent writes.
3. **Align before coining.** Run `align_concepts` (or `suggest_mappings`) before introducing a
   new term, so the graph doesn't drift.
4. **Correct, don't duplicate.** `supersede_decision` when there is a replacement;
   `retract_decision` to deprecate one without a successor. Never silently duplicate — recall
   (list-all) first to confirm a record is genuinely new.
5. **Validate.** Run `validate_against_architecture` after capturing; resolve violations.

### Tool-selection ladder (cheap → precise)
- `get_relevant_context` — fast, deterministic, **shallow** lexical anchor/browse. Start here.
- `query` — walk-planned, synthesized natural-language answer **with a reasoning trace**. Use
  when you need reasoning over relationships, not just a label match. Keep questions short and
  focused (one question per call).
- `sparql` — exact, deterministic structural reads of the graph. Use for precise listings.

### Capture kinds
`ArchitecturalDecision` (the default — a choice + why + what was rejected) ·
`Constraint` (a hard limit/invariant) · `Requirement` (a goal/need) ·
`Pattern` (a deliberate recurring approach) · `AntiPattern` (something to avoid, + why) ·
`Lesson` (a non-obvious learning/gotcha).
<!-- moosedev:end -->
