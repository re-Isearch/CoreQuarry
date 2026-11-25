# CoreQuarry Execution Manual for Autonomous Agents

You are an AI Agent interacting with CoreQuarry—a local-first, coordinate-based hybrid knowledge retrieval engine. Unlike traditional databases, CoreQuarry does not use flat-vector chunking. It treats documents as structured, multi-dimensional physical maps where location and position matter.

Use this manual to construct valid queries and interact with the engine via its Command Line Interface (CLI).

---

## 🛠️ CLI Execution Basics
You must interact with the engine exclusively through the compiled `quarry` binary, typically located at `./ib/bin/quarry`.

### Primary Search Command Structure
```bash
./ib/bin/quarry search -d /path/to/database [formatting_flags] [query_mode] "your_query"
```

### Agent Mode (`-agent`)
Use the **singular** flag `-agent` (not `-agents`) for an evidence-oriented, persistent search session. It is a preset for existing Isearch options, equivalent to:

* `-shell` — start the interactive IRSET shell, retaining result sets and permitting stack operations across queries.
* `-t -tab` — output compact tab-delimited ranked rows without the ordinary result header.
* `-show` — include the best hit neighborhood as evidence.
* `-advice 500` — allow up to 500 characters of evidence context (instead of the usual 130).

For example, start the shell with an initial query:

```bash
./ib/bin/quarry search -d /path/to/database -agent "to be or not to be"
```

Or omit the initial query and submit searches at the shell prompt with `=query`. Within the shell, use `push`, `pop`, `peek`, `size`, `depth`, `AND`, `OR`, `PEER`, `next`, `previous`, `increment N`, and `range` to inspect and combine IRSETs without reopening the database. Enter `quit` to exit. An empty line does not repeat the current results.

The tabular output contains ranked result rows followed by `# Evidence:` comments; whitespace in the evidence is packed into **one physical line** per hit neighborhood. It is deliberately compact and readable, **not** a JSON document. By default diagnostic logging goes to stderr rather than contaminating machine-oriented stdout; an explicit `-log` destination takes precedence. The interactive shell prompt is still printed to stdout, so clients consuming a live session must account for it.

`-agent` **does not** force RPN or XML: the ordinary smart-query interpretation remains the default, and `-rpn` or `-infix` can be requested explicitly. In the shell, `set XML` switches to XML-style results and disables tabular formatting.

`-noshell` explicitly turns off IRSET shell mode (which is otherwise off by default); it does **not** mean `-q`. Use `-q` to print results and exit immediately when not in shell mode. Options are processed in order, so `-agent -noshell -q` gives a one-shot query with the agent formatting preset.

### Essential Formatting Flags
* `-agent` : Recommended for repeated, evidence-oriented searches and IRSET shell operations.
* `-tab` : Tab-delimited ranked results; with `-show`, evidence is a single `# Evidence:` comment line.
* `-XML` : XML-like result representation with explicit hit/container coordinates when detailed machine parsing is needed.
* `-show` : Reconstruct and print the best hit neighborhood around matching text coordinates.

**JSON result output is not implemented yet.** Do not request `-Json` or build a parser that expects it.

---

## 🧩 Dynamic Operator & DataType Discovery
CoreQuarry supports dozens of advanced query operators and strict data types. To prevent token bloat, do not guess or halluncinate syntax. Run these diagnostic flags to dynamically retrieve full list references:

* **To discover all query algebra operators:** `./ib/bin/quarry -qhelp=json`
* **To discover command-line arguments:** `./ib/bin/quarry search -help=json`
* **To discover database configuration options:** `./ib/bin/quarry -ohelp`

---

## 📐 How to Use the Retrieval Algebra
CoreQuarry shifts the paradigm from simple query-response text matches to a programmable retrieval algebra. You must actively program the retrieval process using its specialized operators:

### 1. Agent-Centric Evaluation Operators
* **`MAYBE`** : Returns records matching both operands when possible; if no common match exists, it gracefully falls back to the larger or higher-scoring result set. Use this to protect your loops from empty context windows.
* **`PROMOTE` / `DEMOTE`** : Dynamically adjusts document scores based on the presence of a secondary context parameter without letting that modifier contribute raw hit evidence.

### 2. Spatial & Positional Operators
Because text has physical spatiality in CoreQuarry, use these operators to navigate document architecture precisely:
* **`WITHIN:<field>`** : Restricts your search bounds exclusively to a named tag, section, or metadata container.
* **`BEFORE[:distance]` / `AFTER[:distance]`** : Selects hits only if terms occur in a specific continuous order within a metric scale of source tokens.
* **`PEER` / `ANCESTOR`** : Navigates non-root structural container bounds to grab contextually connected elements (e.g., matching text inside the same paragraph element).

---

## 🤖 Step-by-Step Retrieval Strategy
When executing a search loop, follow this process:
1. **Discover:** Run `./ib/bin/quarry -qhelp=json` to check your query algebra vocabulary.
2. **Query:** Build an explicit, structured query string using positional operators (e.g., using `-rpn` for Reverse Polish Notation or `-infix` for structural logic).
3. **Parse:** Read the `-agent` tabular result rows and single-line `# Evidence:` comments for iterative retrieval. Use `-XML` (or `set XML` in the shell) when exact hit/container coordinates are required; JSON results are not yet available.
4. **Refine:** Do not re-embed or re-chunk text blindly. Use CoreQuarry's structural constraints (`NARROW`, `PEER`, etc.) to narrow or redirect your next query strategy recursively from the existing index.

