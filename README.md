## This is the respositiory for CoreQuarry (re-Isearch+Schmate)


   Copyright 2026 Edward C. Zimmermann, NONMONOTONIC Networks, Munich, Germany
   <http://www.nonmonotonic.net>
      
   Licensed under the Apache License, Version 2.0 (the "License");
   you may not use this file except in compliance with the License.
   You may obtain a copy of the License at

       <http://www.apache.org/licenses/LICENSE-2.0>
      
   Unless required by applicable law or agreed to in writing, software
   distributed under the License is distributed on an "AS IS" BASIS,
   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
   See the License for the specific language governing permissions and
   limitations under the License.

## Description

While the Industry Consensus clamours towards Multi-Gigawatt datacenters, trillion-dollar market caps, and filling massive warehouses—if not actual outer space—with endless arrays of power-hungry GPUs. Everyone seems busy trying to figure out how to nuclear-power a cluster of 100,000 GPUs just to parse human intent. Scale at all costs.

We, by contrast, are looking the exact opposite way. We want to know: how much production-grade retrieval performance can one extract from the bare metal sitting right in front of us, off-the-grid and entirely sovereign?

The Goal: Squeezing maximum structural intelligence, deterministic precision, and state-of-the-art neural intent out of local, edge, and consumer hardware. 

<IMG ALIGN="Right" SRC="logos/CoreQuarry_isometric_logo.svg" ALT="CoreQuarry" height=150>
CoreQuarry is a return to sane systems engineering: maximizing localized hardware to achieve identical semantic depth and absolute structural precision without a cloud tether.

CoreQuarry is not a vector data or RAG framework but a knowledge excavation platform build around a novel hybrid knowledge retrieval engine which emerged in 2026 from Project Schmate (שמאטע) for re-Isearch. It unifies lexical, structural, and semantic search into a single, high-performance platform. Unlike existing vector databases or traditional search engines, it supports true positional indexing, structure-aware queries, and typed object retrieval, enabling precise and contextually-aware search over heterogeneous document corpora. By leveraging memory-mapped, append-only indexes and a two-tier address-based caching system, the engine achieves extremely low memory footprints while scaling to handle complex, hybrid RAG queries on consumer hardware, including laptops and edge devices. 

* CoreQuarry uses positional ranking rather than BM25-based ranking (which is just a Bag-of-Words retrieval model summing individual term contributions without concern for their position withing a corpus). Because BM25 lacks positional awareness, it cannot distinguish between different meanings that depend on word order or positional context.

* Structure-Preserving Indexing: Unlike conventional vector databases and many RAG systems that flatten documents into chunks or JSON records, CoreQuarry preserves document structure during indexing. Sections, headings, positional relationships, metadata, object types, and document hierarchies remain available to the query engine.

See our Constitution/Manifesto (in docs/CoreQuarryManifesto.pdf)


## Uses

This engine enables privacy-preserving, local-first AI retrieval, supporting applications in legal research, medical knowledge, industrial edge AI, and large-scale document analytics. Its small footprint and high flexibility make it uniquely suited for embedded devices, laptops, and offline environments, dramatically lowering the barrier for organizations to implement robust AI-powered search without relying on cloud services.

* No Vendor Lock-In: Reliance on a cloud provider risks catastrophic disruption if the vendor raises prices, alters their API, or goes out of business.

* Immutable Infrastructure: On-prem code bases can be frozen in time, ensuring that search and retrieval tools remain functional and identical for decades.

* Offline Resiliency: Many services must function during regional internet outages or infrastructure failures; local setups ensure internal search operations never go offline.

* Historical Language Tuning: Local open-source models can be custom-tuned to read archaic spellings, dead languages, or specific regional dialects without cloud filtering mechanisms blocking the content.

* Algorithmic Transparency: Researchers require objective, un-biased search results; local open-source retrieval pipelines allow staff to inspect the exact retrieval math, proving how an answer was generated.

* Designed and optimized for COTS unlike enterprise data center hardware which demand three-phase industrial power grids and specialized server room water chillers.

CoreQuarry is designed to enable highly capable local AI systems with a fraction of the energy consumption of conventional cloud deployments. By enabling operation at any point on the retrieval quality–efficiency frontier, CoreQuarry allows organisations to optimise not only for accuracy and cost, but also for energy consumption, deployment constraints, and digital sovereignty requirements.

## Code / Repro

<https://github.com/re-Isearch/CoreQuarry>


## This Repository 

This is the main central repository for CoreQuary (re-Isearch) development.

Its builds on three (actually four projects) of our projects: ib (re-Isearch), bert.cpp (our refactored bert.cpp), Schmate (which includes our HNSWlib fork).

bert.cpp in turns builds on the ggml tensor library.

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


<IMG SRC="https://nlnet.nl/image/logo_nlnet.svg" ALT="NLnet Foundation" height=100> <IMG SRC="https://nlnet.nl/logo/NGI/NGIZero-green.hex.svg" ALT="NGI0 Search" height=100> &nbsp; &nbsp; <IMG SRC="https://ngi.eu/wp-content/uploads/sites/77/2017/10/bandiera_stelle.png" ALT="EU" height=100>


