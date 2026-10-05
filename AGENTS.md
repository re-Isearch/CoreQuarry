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

### Essential Formatting Flags
When calling the CLI, always append one of these flags so you can systematically parse the response:
* `-Json` : Returns structured search results in a clean JSON payload. **(Recommended for parsing)**
* `-XML` : Returns results wrapped in an XML-like hierarchy.
* `-show` : Reconstructs and prints the best hit neighborhood context around the matching spatial text coordinates.

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
3. **Parse:** Consume the `-Json` output to identify the exact coordinates and shapes of information.
4. **Refine:** Do not re-embed or re-chunk text blindly. Use CoreQuarry's structural constraints (`NARROW`, `PEER`, etc.) to narrow or redirect your next query strategy recursively from the existing index.

