# Grandine Lean Client

High-Performance Rust Implementation of Ethereum's Lean Consensus Client

## Motivation

Grandine's lean client is built for Lean Ethereum's post-quantum consensus layer and currently trails most other implementations on devnet 5 (per [hive](https://hive.leanroadmap.org)), despite Grandine's beacon client recently topping minority-client performance benchmarks. The gap needs to be closed before devnet 6 as a high-priority target for the lean client.

## Project description

The proposed work is a focused push to bring Grandine's Lean Client to spec and performance parity with the leading implementations on the hive interoperability suites, and to keep it there as devnet 6 raises the bar.

## Specification

The following test suites have failing cases for Grandine on the hive dashboard:

| Suite | Description | Current Status |
|---|---|---|
| client-interop | Cross-client interoperability checks | 4/52 |
| rpc-compat | RPC API conformance | 28/34 |
| reqresp | Req/Resp protocol messaging | 3/6 |
| lean-spec-tests-fork-choice | Fork-choice spec conformance | 1/123 |
| lean-spec-tests-state-transition | State-transition spec conformance | 13/74 |
| lean-spec-tests-verify-signatures | Signature verification spec conformance | 0/3 |
| Total | | 49/292 |

## Roadmap

### Phase 0: Scoping and Understanding (week 6 - week 8)
- Study more about the lean specs and new devnet specs, and compare with Grandine to understand current gaps.
- Go through all suites on hive to understand failing suites
- Run the hive test suite for Grandine locally and have a note ready with all the actual test failures.
- Examine and solve the root cause of test suites exiting early for Grandine only
- After phase 0, the hive dashboard should be very reliable for checking which suites are failing and need to be worked on.
- Currently Grandine passes only 61 suites, however after this phase itself it can reach 100+ or even 200+ passing suites, since the hive workflow for Grandine itself is broken.

### Phase 1: Execution (week 8 - week 16)
- After phase 0, we do not need to run hive tests locally and can depend on the hive dashboard, easing things up a lot.
- In this phase the test suites are fixed, one by one.
- The order of suites to be fixed can be decided during this phase, as the suites are not interdependent, with a few exceptions.
- The highest priority will be given to making the tests pass successfully, which this phase focuses on.
- Devnet 6 may land during this phase, hence some time will be needed to research into that and prepare for interop testing on the new devnet.

### Phase 2: Performance Optimization (week 16 - week 20)
- Perform extensive interop tests with different clients and make a report on performance metrics (benchmarking)
- Use tools like [leanBench](https://github.com/leanEthereum/leanBench) and [leanMetrics](https://github.com/leanEthereum/leanMetrics) to measure the performance and metrics while running.
- Have discussions with the Grandine team and refactor code to a more efficient version wherever applicable

### Phase 3: Beyond EPF (week 20+)
- Continue working on the lean client in the same way for future devnets

## Possible challenges

- **Moving spec.** Lean Consensus is still under active research; fixes made against devnet 5 semantics may need rework as devnet 6 (if available) change consensus or aggregation parameters.
- **Limited reference material.** Sparse documentation and smaller community around lean clients compared to mainnet consensus clients means more time spent reading other clients' source to resolve ambiguity.
- **Correctness vs. performance tradeoff.** Optimizing too early risks masking spec-compliance bugs; the phased approach must be followed strictly to avoid this.

## Goal of the project

The goal of the project is to have Grandine catch up with the clients leading the lean client race. Passing 250+ test suites sounds like a really good point, as the top 4 clients right now are in the 250 - 300 range. That is the primary goal of the project. The secondary goal is to stick to Grandine's motto of being an efficient client.

If devnet 6 gets introduced while work on the devnet 5 suites is still ongoing, then we would need to work on that too so that Grandine can be in the top 4 clients for devnet 6-only specs from the start.

Basically, the end state is a Grandine Lean Client that reliably interoperates with the rest of the multi-client lean ecosystem and is competitive on performance, closing the gap that currently exists on hive, and leaving the client in a state where continued devnet participation (rather than catch-up) is the norm. The broader impact is a more credible, diverse implementation contributing to client diversity on Ethereum's post-quantum consensus redesign at a stage where interop and performance issues are still cheap to fix.

## Collaborators

### Fellows 

* **Sagar Rana** ([@SoarinSkySagar](https://www.github.com/SoarinSkySagar))
* **Sameer Agarwal** ([@SamAg19](https://github.com/SamAg19))

We will be pursuing this work together and divide work as we see fit.

### Mentors

* **Saulius Grigaitis** ([@sauliusgrigaitis](https://github.com/sauliusgrigaitis)) - Lead, Grandine Core Team

## Resources

- [Grandine Lean Client](https://github.com/grandinetech/lean)
- [Lean Ethereum Roadmap](https://leanroadmap.org)
- [Lean Clients Progress Tracker](https://hive.leanroadmap.org/)
- [Lean Specs](https://github.com/leanEthereum/leanSpec)
- [Utility tool to start local devnet of multiple lean clients](https://github.com/blockblaz/lean-quickstart)
- [Ream Study Group to Learn about PQ Cryptography and zkVM](https://github.com/ReamLabs/ream-study-group)