# pending-stuff review branch

This integration branch pins the `pending-stuff` commits of `ib` and `Schmate`. `bert.cpp` and `ggml` remain unchanged.

**Included**
- `ib`: complete PEER-resolution reuse with coordinate/cursor guards, invalidation and tests (ported from `attrlist-field-type-state`); geometric growth for `FCHITS::Append` (covered by `typed_search`).
- `Schmate`: floating PASS distance and row-alignment correction (ported from `query_energy`), regression tests; INT4 NEON/AVX2/scalar packing parity, FP16 bit-storage correction, and new standalone parity test.

**Validation (not run in both architectures here):**
```sh
# Configure ib with IB_BUILD_SCORING_TESTS=ON:
ctest --test-dir <ib-build> --output-on-failure -R 'peer_resolver|typed_search'

# From Schmate:
cmake -S tests/q8 -B build-pass && cmake --build build-pass
ctest --test-dir build-pass --output-on-failure
bash tests/run_energy_tests.sh
```

Run SIMD parity on x86 default, x86 with AVX2, and AArch64 NEON; also exercise floating PASS index persistence and reload. This is source-ported review work, not yet native-build validated.

**For follow-up:** older INT8/INT16 SIMD helpers in `int_storage.h` are not currently dispatched by `IntStorage::quantize`, and have differing unsigned / rounding behavior. Do not enable without additional parity tests. Scalar FP16 edge rounding/subnormal encoding also merits audit.

**Excluded on purpose:** ZUGFeRD/Factur-X (potential doctype plug-in, dependency decision pending). Interactive shell and Energy functionality are already in main.

After switching to `CoreQuarry/pending-stuff`, use `git submodule update --init --recursive` to fetch its recorded review commits; `--remote` would follow the configured `main` branches instead.
