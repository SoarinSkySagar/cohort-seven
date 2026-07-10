# Grandine Lean Client

High-Performance Rust Implementation of Ethereum's Lean Consensus Client

## Motivation

Lean Ethereum is a full redesign of Ethereum's consensus, data, and execution layers, Ethereum's most ambitious protocol overhaul since the Merge. Its consensus piece, Lean Consensus, replaces the Beacon Chain design with one built around post-quantum security: current BLS signatures rely on elliptic-curve math that a sufficiently powerful quantum computer could eventually break, so Lean Consensus moves to hash-based signature schemes that stay secure against both classical and quantum adversaries. These hash-based primitives are SNARK-friendly, enabling efficient proof aggregation, so the same building block covers both quantum-resistance and performance.

Grandine is a lean client developed by Grandine beacon client developers for connecting to Lean Ethereum and contributing to the consensus layer. Like the beacon client, the lean client is also written in Rust and prioritizes performance and efficiency over everything else. A few days back, the Grandine beacon client achieved top performance out of all the minority clients. The lean client aims to achieve a similar milestone.

However, currently the Grandine client falls behind most of the other clients on [hive](https://hive.leanroadmap.org/?devnet=devnet5) on devnet 5 suites. With devnet 6 and devnet 7 also in the works, Grandine needs to catch up to the lean roadmap quickly.

## Project description

The proposed work is a focused push to bring Grandine's Lean Client to spec and performance parity with the leading implementations on the hive interoperability suites, and to keep it there as devnet6 and devnet7 raise the bar.

## Specification

### 1. Gap analysis against hive suites
- Run Grandine against the current [hive devnet suites](https://hive.leanroadmap.org/?devnet=devnet5) and categorize failures by subsystem: state transition, fork choice, signatures/aggregation, networking, sync, RPC.
- Diff Grandine's [leanSpec](https://github.com/leanEthereum/leanSpec) STF implementation against spec and passing clients to isolate compliance gaps vs. bugs.
- Produce a prioritized fix list on `grandinetech/lean`, ranked by hive suite impact.

### 2. Implementation
- Work on the ranked fix list and steadily increase the number of passing suites on the Hive roadmap
- Keep an eye on performance benchmarks and ensure the "performance first" philosophy holds.

### 3. Performance optimization and continuous validation
- Profile via `lean-quickstart` and [leanBench](https://bench.leanroadmap.org/); apply Grandine's mainnet-proven performance patterns (parallelized validation, efficient SSZ hashing) where applicable.
- Re-run hive suites and leanBench after each pass, and re-triage as devnet6/devnet7 (if available) specs land, tracking Grandine's hive ranking as the core progress signal.

## Roadmap

### Phase 0: Scoping and Understanding
- Study more about the lean specs and new devnet specs, and compare with Grandine to understand current gaps.
- Go through all suites on hive to understand failing suites

### Phase 1: Implementation
- Implement the missing diffs found in Phase 0, and ensure the relevant suites pass

### Phase 2: Performance Optimization
- Optimize implementations for better CPU and RAM performance under stress

### Phase 3: Testing and Debugging
- Extensive stress testing and seeing how the client holds under massive loads
- Debug all issues found


## Possible challenges

- **Moving spec.** Lean Consensus is still under active research; fixes made against devnet5 semantics may need rework as devnet6/7 (if available) change consensus or aggregation parameters.
- **Limited reference material.** Sparse documentation and smaller community around lean clients compared to mainnet consensus clients means more time spent reading other clients' source to resolve ambiguity.
- **Correctness vs. performance tradeoff.** Optimizing too early risks masking spec-compliance bugs; the phased approach must be followed strictly to avoid this.

## Goal of the project

Success looks like Grandine's Lean Client passing >80% [hive interoperability suite](https://hive.leanroadmap.org/?devnet=devnet5) on par with the leading lean clients, sustained across the devnet5 → devnet6 → devnet7 (if available) progression rather than achieved once and left to regress.

In scope by the end of the project:
- All known spec-compliance gaps in state transition, fork choice, and signature/aggregation logic are resolved and verified against cross-client SSZ test vectors.
- Grandine's metrics and networking stack are fully aligned with the ecosystem's shared conventions, so it participates in multi-client devnets and shared tooling (`lean-quickstart`, `leanpoint`, Grafana dashboards) without custom workarounds.
- Performance optimizations are applied and benchmarked on [leanBench](https://bench.leanroadmap.org/), bringing Grandine's lean client closer to the performance profile of its mainnet counterpart.

The end state is a Grandine Lean Client that reliably interoperates with the rest of the multi-client lean ecosystem and is competitive on performance — closing the gap that currently exists on hive, and leaving the client in a state where continued devnet participation (rather than catch-up) is the norm. The broader impact is a more credible, diverse implementation contributing to client diversity on Ethereum's post-quantum consensus redesign at a stage where interop and performance issues are still cheap to fix.

## Collaborators

### Fellows 

* **Sagar Rana** ([@SoarinSkySagar](https://www.github.com/SoarinSkySagar))

### Mentors

* **Saulius Grigaitis** ([@sauliusgrigaitis](https://github.com/sauliusgrigaitis)) - Lead, Grandine Core Team

## Resources

- [Grandine Lean Client](https://github.com/grandinetech/lean)
- [Lean Ethereum Roadmap](https://leanroadmap.org)
- [Lean Clients Progress Tracker](https://hive.leanroadmap.org/)
- [Lean Specs](https://github.com/leanEthereum/leanSpec)
- [Utility tool to start local devnet of multiple lean clients](https://github.com/blockblaz/lean-quickstart)
- [Ream Study Group to Learn about PQ Cryptography and zkVM](https://github.com/ReamLabs/ream-study-group)