# CoreQuarry

<IMG ALIGN="Right" SRC="logos/CoreQuarry_isometric_logo.svg" ALT="CoreQuarry" height=150>

*A search and retrieval engine built to run on the hardware you already own.*

Source: <https://github.com/re-Isearch/CoreQuarry> · Licensed under the [Apache License 2.0](#license)

**Contents:** [What it is](#what-it-is) · [Why local-first](#why-local-first) · [What you get](#what-you-get) · [Where it's used](#where-its-used) · [Quickstart](#quickstart) · [Why it works this way](#why-it-works-this-way) · [What this repository is built from](#what-this-repository-is-built-from) · [Building, installing and developing](#building-installing-and-developing) · [Learn more](#learn-more) · [Thanks](#thanks)

## What it is

CoreQuarry is a search and retrieval engine. Point it at a folder of documents, PDFs, XML, JSON, email, source code (or whatever you have), and it indexes them without needing to flatten everything into a generic blob. It keeps track of where things sit inside a document: which paragraph, which field, which speaker, which record.

That structure stays available at query time, so a search can return a phrase, the section it appeared in, the person who said it, or the record it belongs to, depending on what's being asked for.

It combines three kinds of search in one engine:
- Keyword search
- Structural search (fields, paths, record boundaries)
- Semantic vector search.

Most tools make you stitch two or three separate systems together to get all of that. CoreQuarry treats them as one problem.

And it runs *entirely on your own hardware*. No cloud service, no API key, no data leaving the building. That isn't a privacy feature bolted on afterwards; it's the reason the engine is built the way it is.

It's also built to be queried by something other than a person. Alongside the usual boolean operators, CoreQuarry adds ones like `PROMOTE`, `DEMOTE`, and `MAYBE`, designed for an LLM or agent to construct and refine a search plan step by step, rather than a human typing one query and reading the results. See [Where it's used](#where-its-used) below.

## Why local-first

Most "AI-powered search" today means sending your documents somewhere else (a hosted vector database, an LLM provider's API, etc.) for chunking and embedding. Which means your data crosses infrastructure you don't control, sits under legal jurisdictions you might not have chosen, and depends on a vendor's pricing and API both staying the same.

CoreQuarry exists to see how much of that dependency is actually necessary. It runs the full hybrid retrieval stack, keyword, structural, and semantic, on a laptop, or on something considerably smaller. We've benchmarked it on a single-board computer that draws about 50 watts.

If your organization needs its search to keep working during a network outage, or needs to be certain that sensitive documents never touch a third party's servers, that's the problem this was built to solve.


## What you get

- **Nothing must leave your infrastructure:** Documents, queries and any embeddings you generate can stay on hardware you control.
- **No vendor to get locked into:** There's no proprietary API to migrate away from if pricing or terms change.
- **Works offline, indefinitely:** Useful if a service needs to survive a regional outage, or simply never has a connection to begin with.
- **Ranking you can inspect:** You can see exactly why a result scored the way it did, rather than trust a black box.
- **Runs on hardware you probably already have:** A laptop is enough to get started; it also runs on edge devices with a fraction of that power.

## Why did we develop this?

The Industry Consensus seems to crave Multi-Gigawatt datacenters, trillion-dollar market caps, and filling massive warehouses—if not actual outer space—with an endless arrays of power-hungry GPUs. From worrying about climate distruption now Everyone seems busy trying to figure out how to nuclear-power a cluster of 100,000 GPUs just to parse human intent. Scale at all costs.

We, by contrast, are looking the exact opposite way. We want to know: how much production-grade retrieval performance can one extract from the bare metal sitting right in front of us, off-the-grid and entirely sovereign?

**The Goal**: Squeezing maximum structural intelligence, deterministic precision, and state-of-the-art neural intent out of local, edge, and consumer hardware. 

CoreQuarry is a return to sane systems engineering: maximizing localized hardware to achieve identical semantic depth and absolute structural precision without a cloud tether.

Retrieval-Augmented Generation (RAG) was introduced by Meta AI in 2020 to solve the issue of Large Language Models hallucinating and lacking up-to-date knowledge. It works by fetching relevant facts from an external database and merging them with the user's prompt, though early versions suffered from fragmentation and retrieval errors, leading to RAG 2.0-- adding lexical search, knowledge graphs and an agentic loop-- and lately some so-called 3.0 architectures (orchestration).  At the heart of most of these systems is a traditional structureless lexical search using an inverted index and score normalization using BM25 or TD-IDF. These systems don't use structure or position since their basic algorithms are inadaquate to the task. They build instead on the notion that a re-ranker-- most commonly Reciprocal Rank Fusion (RRF)--  can make up for the loss.  We advocate, by contrast, deterministic, context-aware structure traversal. The engine should treat  a document not as an arbitrary bag of words or vector space, but as a rigid multi-dimensional map.  A positional index acts like a persistent physical grid. It allows the generation engine to mathematically trace a fact back to its coordinates. Location-storing structural engines can map the exact geometric coordinate of every string, completely eliminating guessed or hallucinated citations.

CoreQuarry, in this sense, is not a vector data or RAG framework but a knowledge excavation platform. It is built around a novel hybrid knowledge retrieval engine which emerged in 2026 from Project Schmate (שמאטע) for re-Isearch. It unifies lexical, structural, and semantic search into a single, high-performance platform. Unlike existing vector databases or traditional search engines, it supports true positional indexing, structure-aware queries, and typed object retrieval, enabling precise and contextually-aware search over heterogeneous document corpora. By leveraging memory-mapped, append-only indexes and a two-tier address-based caching system, the engine achieves extremely low memory footprints while scaling to handle complex, hybrid RAG queries on consumer hardware, including laptops and edge devices. 

Our design paradigm is especially suited to the emerging next generation of **LLM** and **RL agents**. Agents need more than keyword matching. They need to express relationships: this term occurs inside that field, these concepts appear near each other, this phrase comes before that phrase, these facts belong to the same structural element. Positional and structural operators let an agent ask about the shape of information, not merely its presence.

LLMs, by contrast, tend to see retrieved text as relatively flat. Once content is turned into tokens, chunks, or embeddings, much of the original document structure—fields, containment, adjacency, order, hierarchy—can become weak or implicit. An LLM may infer those relationships, but inference is not the same as querying them precisely. Information signals for context are literally thrown out but to have downstream processes try to re-infer what may have originally been there.

Structural and positional search gives, by contrast, an agent a way to preserve that information before generation: retrieve the right relationship, not just the right words. For agentic search, operators such as NEAR, BEFORE, PEER, WITHIN, and field constraints are therefore not syntactic luxuries; they are tools for turning a flat language model into a much more precise information-seeking system.

CoreQuarry is built to act as the mathematically precise, structurally permanent bedrock underneath whatever model generation layer you choose to run. It tracks exact physical positions of terms, structures (implicit and explicit via SGML, XML, JSON etc), and document schemas. An agent looking for an exact structural match gets a perfect algorithmic return, while the Schmate sub-engine handles the semantic search alongside it.

- This paradigm lets the agent actively construct and refine expressive retrieval plans using Boolean, structural, positional, scoring, and relaxation operators.
- The key idea is that the agent is no longer just consuming retrieved context. It is programming the retrieval process:
> "RAG gave LLMs documents. CoreQuarry  gives agents a retrieval algebra."


To this end:

* CoreQuarry uses positional ranking rather than just BM25-based ranking (which is merely a Bag-of-Words retrieval model summing individual term contributions without concern for their position withing a corpus). Because BM25 lacks positional awareness, it cannot distinguish between different meanings that depend on word order or positional context.

* Structure-Preserving Indexing: Unlike conventional vector databases and many RAG systems that flatten documents into chunks or JSON records, CoreQuarry preserves document structure during indexing. Sections, headings, positional relationships, metadata, object types, and document hierarchies remain available to the query engine.

See our Constitution/Manifesto (in docs/CoreQuarryManifesto.pdf)

<PRE>
CoreQuarry / IB
    authoritative documents
    fields / GP coordinates
    deletion state
    persistence semantics
             |
             v
Schmate
    model management
    text reconstruction
    embedding/chunk semantics
    shard management
    metadata + model identity
    vector persistence/rescoring
    synchronization / GC
             |
             v
modified HNSWLIB
    graph
    ANN traversal
    filtering/deletion hooks
    richer vector representations
    quantization/storage-aware spaces
    CPU/vectorized distance machinery
</PRE>


## Where it's used

- **Grounding an LLM or RAG pipeline** with an exact passage instead of an arbitrary 500-byte chunk. Because CoreQuarry indexes structure rather than just text, it can hand back "the third paragraph of section four" instead of whatever window a generic chunker happened to cut.
- **Agentic, recursive search.** An agent can issue a query, look at what came back, and narrow or redirect the next one using CoreQuarry's own operators, rather than re-embedding and re-querying a vector store from scratch each time. That's a different shape of use than a person typing one search and reading the results, and it's the direction most of the engine's recent development has gone.
- **Large scientific and geospatial datasets.** Earlier versions of this engine's search core have indexed national genomic archives and observatory data, where records rarely look like tidy prose.
- **Legal, government and archival collections**, where knowing precisely where a result came from matters as much as finding it.
- **Edge and embedded deployments** with no spare GPU and no guaranteed network connection: field equipment, offline research stations, point-of-sale systems.
- * Immutable Infrastructure: On-prem code bases can be frozen in time, ensuring that search and retrieval tools remain functional and identical for decades.
- * Offline Resiliency: Many services must function during regional internet outages or infrastructure failures; local setups ensure internal search operations never go offline.

CoreQuarry is designed to enable highly capable local AI systems with a fraction of the energy consumption of conventional cloud deployments. By enabling operation at any point on the retrieval quality–efficiency frontier, CoreQuarry allows organisations to optimise not only for accuracy and cost, but also for energy consumption, deployment constraints, and digital sovereignty requirements.

## Code / Repro

<https://github.com/re-Isearch/CoreQuarry>


## This Repository 

This is the main central repository for CoreQuary (re-Isearch) development.

Its builds on three (actually four projects) of our projects: ib (re-Isearch), bert.cpp (our refactored bert.cpp), Schmate (which includes our HNSWlib fork). While they have been designed to be used as part of a unified and complete algebraic knowledge extraction engine, its parts are also fully useable in and of themselves.

- **IB**:  the search kernel, provides the indexing, search and document handling services. It contains lexical and object (such as numerical etc.) indexes as well as interfaces to external stores.  It can be built and used with or without vector addtions (Schmate).

- **Schmate**: The vector DB engine. It builds on our own bert.cpp, the standard llama.cpp as well as on our fork on the HNSWlib.

- **HSNWlib**: our fork of Markov's HNSWLIB signficantly enhanced and turbo-charged for ARM. This is officially a sub-project of Schmate..

- **bert.cpp**:  This module provides the BERT services for embeddings. It in turn builds on the GGML tensor library.

Underneath bert.cpp/llama.cpp is the **GGML tensor library**. It is an open source community driven machine learning (ML) library currently centered around HuggingFace. It is written in C/C++ with a focus on Transformer inference on bare metal COTS hardware. We choose to build on ggml rather than ONNX Runtime for its distinct advantages in our use case-- not least of which its performace and efficiency.

## Building, Installing, and Developing

To clone the project along with the absolute latest versions of all internal submodules (`Schmate`, `bert.cpp`, and `ib`), run:

```bash
git clone --recurse-submodules --remote-submodules git@github.com:re-Isearch/CoreQuarry.git
```

### 1. Build Requirements (GGML Layout)
This project relies on `ggml`. The submodules (like `bert.cpp`) look for `ggml` using a relative symlink pointing up to the root application folder. 

To ensure the build system can resolve headers and objects, clone `ggml` directly into the `CoreQuarry` root directory alongside your submodules:

```bash
# Ensure you are in the CoreQuarry root folder
cd CoreQuarry

# Clone ggml so your submodule symlinks point to the right place
git clone https://github.com/ggml-org/ggml.git
```

### 2. Compilation
We use CMake for our build system. Create a build subdirectory to compile the project (easier said than done!):

We expect a compiler that support C++17+

```bash
mkdir build && cd build
cmake ..
make
```

### 3. Model Installation Paths
Our software looks for `.gguf` models in specific directories depending on your operating system and configuration.

#### System-Wide Sharing (Linux)
Models are expected to be stored in `/opt/models/gguf`. To share access across multiple local development users without permission errors, run:

```bash
# Create a dedicated group for managing models
sudo groupadd aimodels

# Add developers to the group (replace <username> with actual names)
sudo usermod -aG aimodels <username1>
sudo usermod -aG aimodels <username2>

# Set up the folder with shared group permissions
sudo mkdir -p /opt/models/gguf
sudo chown -R :aimodels /opt/models/gguf
sudo chmod -R 775 /opt/models/gguf
```
*(Note: Users must log out and log back in for group changes to take effect).*

#### System-Wide Sharing (macOS)
Models are expected to be stored in `/Users/Shared/Models/gguf`. Set up the directory with shared local permissions by running:

```bash
mkdir -p /Users/Shared/Models/gguf
chmod -R 775 /Users/Shared/Models/gguf
```

#### User-Specific Models (Linux, Unix, macOS)
If you do not want to install models system-wide, you can place them inside your user home directory instead. The application will automatically check:

```bash
~/.ib/models/
```

## 📦 Submodules & Dependencies

This project uses `bert.cpp`, `Schmate`, and `ib` as submodules. 

### First-time Setup
To clone this repository along with all of its required submodules, use:
```bash
git clone --recursive <your-repository-url>
```

### Keeping Everything Up to Date
Because the submodules are updated frequently, running a standard `git pull` on this base repository might leave you with obsolete submodule code. 

To forcefully pull the absolute latest updates from the `main` branches of all subprojects, run:
```bash
git submodule update --remote --merge
```

## Thanks

This project was made possible:

- Through the NGI0 Commons Fund, a fund established by NLnet with financial support from the European Commission's Next Generation Internet programme, under the aegis of DG Communications Networks, Content and Technology under grant agreement No 101135429. Additional funding is made available by the Swiss State Secretariat for Education, Research and Innovation (SERI).

- Through a Grant from the Bundesministerium für Forschung, Technologie und Raumfahrt (Germany) GRANT_NUMBER: 01IS22S32 (exploring support of the IPFS and supporting remote indexing).

- Through OpenData CH/Mercator Foundation CH.

- Through a grant from the European Commission Coordination and Support Action (CSA) on ICT standardisation (extending support for additional post ISO-8601:2019 features).

- Through the NGI0 Discovery Fund, a fund established by NLnet with financial support from the European Commission's Next Generation Internet programme, under the aegis of DG Communications Networks, Content and Technology under grant agreement No 825322

- Additional thanks to ETH Zurich SPH who housed ExoDao Network Association 2022-2025 and Amazon AWS who provided a generous hosting grant.


<IMG SRC="https://nlnet.nl/image/logo_nlnet.svg" ALT="NLnet Foundation" height=100> <IMG SRC="https://nlnet.nl/logo/NGI/NGIZero-green.hex.svg" ALT="NGI0 Search" height=100> &nbsp; &nbsp; <IMG SRC="https://ngi.eu/wp-content/uploads/sites/77/2017/10/bandiera_stelle.png" ALT="EU" height=100> <IMG SRC="https://upload.wikimedia.org/wikipedia/commons/f/f3/Flag_of_Switzerland.svg" height=100>

