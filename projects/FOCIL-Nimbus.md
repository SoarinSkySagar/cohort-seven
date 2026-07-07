# Nimbus CL FOCIL Implementation

Implementing Fork-Choice Enforced Inclusion Lists in Nimbus Consensus Layer client

## Motivation

Fork-Choice Enforced Inclusion Lists (FOCIL, [EIP-7805](https://eips.ethereum.org/EIPS/eip-7805)) is a protocol upgrade that enhances censorship resistance in Ethereum. Under this mechanism, a distributed committee of validators, known as Inclusion List Committee, prepares a list of transactions directly from their local mempools. These lists of transactions are stored into respective Inclusion Lists (ILs) by every member of the committee.

Block proposers are required to include the transactions from these ILs in the next block. If a proposer omits the transactions from the ILs, the attesters will refuse to vote for the block, preventing it from becoming canonical.

By tightly coupling transaction availability with fork-choice enforcement, FOCIL shifts the power of transaction selection back to a decentralized committee without disrupting the block production pipeline.

## Project description

The [Nimbus Consensus Client](https://github.com/status-im/nimbus-eth2) does not yet have an implementation of FOCIL. The goal of this project is to assist the Nimbus team in implementing and validating the FOCIL spec, with the fellows primarily contributing to the testing and debugging as the core implementation progresses.

## Specification

The FOCIL implementation includes several components:

### 1. SSZ Containers
FOCIL introduces two main SSZ structures:
* `InclusionList`: Contains metadata and transactions.
* `SignedInclusionList`: Wraps the inclusion list message and signature.

### 2. Validator Client Changes
* `get_inclusion_list_committee`: Helper function to retrieve the inclusion list committee members for a given slot.
* `is_inclusion_list_committee_member`: Helper function to check if a validator is part of the committee.
* **Inclusion List Duties**: New duties for validator client to monitor the mempool, compile transaction batches, sign the `InclusionList`, and broadcast it.

### 3. P2P Networking
* Gossip Topic: New subnet configuration to handle broadcasting of `SignedInclusionList` messages.
* `validate_signed_inclusion_list`: Gossip validation logic to check signatures, slot timing, committee membership, and payload sizes.
* (`InclusionListsByRoot` / `InclusionListsByRange`): RPC methods to retrieve missing inclusion lists from peers.

### 4. Engine API & Execution Layer Changes
* `engine_newPayloadV5`: Extended method carrying `inclusionListTransactions` to EL to enable proposer to propose blocks with FOCIL transactions.
* `engine_forkchoiceUpdatedV4`: Extended method to support passing inclusion list constraints to the block builder.

### 5. Fork-Choice & Attestation Changes
* `get_head` modification: Updated head selection algorithm to enforce that only blocks satisfying FOCIL constraints are eligible canonical heads.
* `is_inclusion_list_satisfied`: Attestation check run by validators before casting votes to verify the proposed block contains all necessary transactions from the inclusion list.

## Roadmap

### Phase 1 (Weeks 6–8): 

* Stay updated with the evolving FOCIL spec by following FOCIL breakout calls and tracking changes in the [consensus-specs repository](https://github.com/ethereum/consensus-specs/).
* Follow the Nimbus team's implementation PRs to build an understanding of the codebase and implementation approach.

### Phase 2 (Weeks 8–10): 

* Set up a local FOCIL devnet using available client images to understand the expected runtime behavior and interop surface before testing begins.

### Phase 3 (Weeks 11–13): 

* Begin active testing and debugging as the Nimbus implementation reaches a testable state. Write spec-aligned test cases, identify and report failures, and contribute fixes where appropriate.

### Phase 4 (Weeks 14–16): 

* Participate in cross-client interop testing on FOCIL devnets alongside other CL and EL implementations, ensuring Nimbus behaves correctly in multi-client environments.

## Possible challenges

* FOCIL is still actively being discussed and might lead to changes in structures, sizes, or other specs.
* Cross-client interop testing requires coordination across multiple client teams which introduces implementation and timeline issues.

## Goal of the project

Success looks like:
* Spec-compliant FOCIL integrated into the Nimbus consensus client, passing all relevant consensus-spec-tests.
* Nimbus passing all interop tests on FOCIL devnets alongside other CL and EL clients.
* Test suite and debugging contributions that help the Nimbus team ship a spec-compliant and mainnet-ready FOCIL implementation.

## Collaborators

### Fellows 

* **Abhivansh** ([@akronim26](https://github.com/akronim26))
* **Sagar** ([@SoarinSkySagar](https://github.com/SoarinSkySagar))

### Mentors

* **Agnish** ([@agnxsh](https://github.com/agnxsh)) - Core Developer, Nimbus Client Team

## Resources

* [EIP-7805: Fork-choice enforced Inclusion Lists (FOCIL)](https://eips.ethereum.org/EIPS/eip-7805)
* [Consensus Specifications](https://github.com/ethereum/consensus-specs)
* [Nimbus Consensus Client Repository](https://github.com/status-im/nimbus-eth2)
* [Nimbus-eth2 focil Development Branch](https://github.com/status-im/nimbus-eth2/tree/focil)
* [PR #7637: FOCIL Spec Draft Implementation](https://github.com/status-im/nimbus-eth2/pull/7637)
* [PR #7290: Precursor Inclusion List Implementation](https://github.com/status-im/nimbus-eth2/pull/7290)
* [PR #7253: SSZ Container Spec Support](https://github.com/status-im/nimbus-eth2/pull/7253)