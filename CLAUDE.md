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


