# MLAsm

This repository is a fork of [`JGalego/MLAsm`](https://github.com/JGalego/MLAsm), an x86-64 Assembly/C machine-learning inference framework.

## Fork provenance

The upstream project provides the original architecture, examples, tests, benchmark harness, and documentation. This fork must not treat upstream benchmark numbers, test counts, CI badges, or memory-safety claims as measurements of the fork unless they are reproduced against the exact fork revision being discussed.

The current fork-specific patch at `16e5598ad8b4870f95cf19b9f413050b87cffd34` makes three scoped changes:

- fixes throughput calculation so `throughput_per_sec` is derived from total predictions and elapsed nanoseconds;
- adds a unit regression that checks throughput/latency unit consistency;
- makes object compilation depend on directory creation so clean parallel builds do not race the output directories.

Those changes do not establish broader performance, leak-freedom, thread-safety, or model-quality claims.

## Source structure

The current tree contains:

- x86-64 NASM sources under `src/`;
- C integration/runtime code under `src/`;
- public headers under `include/`;
- unit, integration, and benchmark programs under `tests/`;
- examples and documentation inherited from upstream.

The Makefile builds with GCC and NASM and enables AVX2/FMA flags for the C side. AVX-512 flags are added when detected by the current Makefile logic.

## Build

Requirements represented by the current Makefile:

- GNU Make
- GCC
- NASM
- `ar`
- `libm`
- an x86-64 host compatible with the enabled instruction flags

```bash
make clean
make all
```

## Verification commands

Run the repository's own gates on the exact revision before making performance or correctness claims:

```bash
make test-unit
make test-integration
make test-performance
```

Optional checks exposed by the Makefile:

```bash
make test-memory   # requires Valgrind
make lint          # requires cppcheck
```

`test-performance` produces measurements for the machine and revision on which it is executed. Historical numbers in upstream documentation are not automatically results for this fork.

## CI boundary

A GitHub Actions workflow is present under `.github/workflows/ci.yml`. The presence of a workflow file is not evidence that a specific commit passed it; verify the Actions result for the commit being evaluated.

## Relationship to Opcode Orchestra

`Grar00t/opcode-orchestra` can use this fork as an optional bridge for generated scene constants. That integration is intentionally outside Opcode Orchestra's core Assembly build and is pinned there to a specific MLAsm revision.

## License

MIT. Upstream authorship and license terms remain applicable; see [`LICENSE`](LICENSE).
