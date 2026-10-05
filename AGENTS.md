# CoreQuarry Agent Instructions
This file defines architecture constraints, build parameters, and rules for AI coding agents modifying CoreQuarry. Always strictly adhere to these boundaries.

## 🛠️ Project Definition & Stack
CoreQuarry is a zero-dependency, C++17 local-first hybrid search and retrieval engine. 
* **Core Engine:** Written in pure C++ (the `ib` sub-project handles lexical/structural; `Schmate` handles vectors).
* **Neural Math:** Runs over raw `ggml` tensors (via `bert.cpp`) targeting bare-metal local acceleration (CUDA, Metal, Vulkan).
* **DO NOT** introduce external database containers (Milvus, Qdrant, Chroma, Docker), heavy multi-layered Python orchestration libraries (LangChain, LlamaIndex), or additional runtime networking/serialization layers.

## 🚀 Build, Test, and Dependency Rules
* **Build System:** Always use CMake.
* **Compilation Workflow (from Root):**
  ```bash
  mkdir -p build && cd build
  cmake ..
  make -j$(nproc)
  ```
* **Binary Locations:** Compiled binary tools output strictly into `./ib/bin/`. The primary command-line tool is `./ib/bin/quarry`.
* **Submodules:** CoreQuarry relies on pinned internal submodules (`ib`, `Schmate`, `bert.cpp`, `ggml`). Never download external dependencies using native package managers (apt, brew). If submodules must be brought up to date, execute:
  ```bash
  git submodule update --remote --merge
  ```

## 🧠 Architectural & Algorithmic Guardrails
CoreQuarry completely rejects modern "flat-vector text chunking." It treats documents as structured, multi-dimensional, continuous physical grids mapped by strict text-coordinate mathematics.
1. **Never implement flattening or text splitting:** Do not write algorithms that break files into clean string snippets or force structural JSON into generic text blocks. 
2. **Preserve Spatial Topomorphy:** The engine preserves absolute physical positions and schemas (headings, paragraphs, annotations, fields). 
3. **Query Syntax Constraints:** CoreQuarry evaluates queries via a unique programmable Retrieval Algebra (supporting structural/positional relations like `BEFORE`, `NEAR`, `WITHIN`, `PEER`, `ANCESTOR`, and `NARROW`). Understand that queries default to Reverse Polish Notation (RPN) or exact structured Infix notation.

## 💻 CLI Integration & Agent Interaction
* System automation and runtime code generation must interact with the engine exclusively through the `quarry` CLI.
* **JSON Output:** When generating scripts or wrapper tools that consume results, always use the structural parsing flag `-Json` to obtain structured semantic returns. 
* **Snippet Extraction:** Use the `-show` parameter to fetch actual underlying text regions mapping to the requested coordinate hits instead of writing arbitrary string buffers in memory.

