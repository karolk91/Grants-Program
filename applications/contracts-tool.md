# Contracts performance messurement tool

- **Gloslab:**
- **Payment Address:** Ethereum (USDC/DAI)
- **[Level](https://github.com/w3f/Grants-Program/tree/master#level_slider-levels):** 1, 2 or 3

## Project Overview :page_facing_up:

### Overview

The key objective of this grant is to build a tool which allows to compare contracts performance and to track its regressions.
Subject of comparision are solidity, and ink! contracts running on parachains (pallet-contract and pallet-evm).
Tool will be integrated with CI and running each night, generating messurements data and providing visualisation.
Our team has strong interest in contracts development on Polkadot and building Polkadot ecosystem.

### Project Details

Project is based on smart-bench Parity repo https://github.com/paritytech/smart-bench and developed in rust language.
Apart of existing ink! and solidity contracts support, will be introduces support for solang compiled contract running on pallet-contract.
Finally the tool will messure performance of:
-Ink! contract run on pallet-contract (currently supported)
-Solidity contracts comiled with solc run on pallet-evm (supported but outdated)
-Solidity contracts compiled with solang and run on pallet-contract (new funtionality)

The tool will be connected to CI, and each night will generate messurements data for contracts and upload them to database.
With the tool comes Visual dashboard based on rustc-perf Site.

The tool will be run with following commands:
cargo run --release -- ink-wasm erc20 erc1155 --instance-count 10 --call-count 20 --url ws://localhost:9988
cargo run --release -- sol-wasm erc20 erc1155 --instance-count 10 --call-count 20 --url ws://localhost:9988
cargo run --release -- evm erc20 erc1155 --instance-count 10 --call-count 20 --url ws://localhost:9988

Messurements data are generated in the shape of:
PoV Size=0130KiB(005%) Weight RefTime=0000088ms(017%) Weight ProofSize=3277KiB(064%) Witness=0118KiB Block=0011KiB NumExtrinsics=0048

Each test is repeated 10 times and data are store in database for further processing.
In DB we store for each run PoV sice in Kib Block Weight(ms, KiB), Witness in KB, Block size in KiB and number of extrisics being processed.

The performance messurements are run against test network, which is setup using zombinet.
Required scripts are delivered with the tool.

Limitations:
the project does not compile the contracts by itself, contracts are delivered in binary form.
the project does not provide server/instance with database and visual dashboard.

### Ecosystem Fit

The project is usefull for ecosystem at contracts development stage to track its performance and regressions on Polkadot.
It is going to be used also to messure ink! language performance by Parity core team.

## Team :busts_in_silhouette:

### Team members

- Sebastian Miasojed
- Karol Kokoszka

### Contact

- **Sebastian Miasojed:** Full name of the contact person in your team
- **s.miasojed@gmail.com:** Contact email (e.g. john@duo.com)
- **Website:** None

### Legal Structure

- **Registered Address:** Address of your registered legal entity, if available. Please keep it in a single line. (e.g. High Street 1, London LK1 234, UK)
- **Registered Legal Entity:** Name of your registered legal entity, if available. (e.g. Duo Ltd.)

### Team's experience

We combine development and architecting skills from embdded world, cloud systems and apply them to crypto world.
Until now the team has shown his profficiency alligning smart-bench with newest libraries required by ink! contracts.

### Team Code Repos

- https://github.com/smiasojed
- https://github.com/<team_member_2>

### Team LinkedIn Profiles (if available)

- hhttps://www.linkedin.com/in/sebastian-miasojed-83b6123/
- https://www.linkedin.com/<person_2>


## Development Status :open_book:

Work has been started, smart bench has been updated with new libraries and is able to build and run on test net with ink! contracts.
https://github.com/paritytech/smart-bench/pull/28
https://github.com/paritytech/blockstats/pull/22

## Development Roadmap :nut_and_bolt:

### Overview

- **Total Estimated Duration:** Duration of the whole project 2 months
- **Full-Time Equivalent (FTE):**  0.5
- **Total Costs:** 

### Milestone 1 Example — Basic functionality

- **Estimated duration:** 1 month
- **FTE:**  0.5
- **Costs:** 

> :exclamation: **The default deliverables 0a-0d below are mandatory for all milestones**, and deliverable 0e at least for the last one. 

| Number | Deliverable | Specification |
| -----: | ----------- | ------------- |
| **0a.** | License | Apache 2.0 / GPLv3 / MIT / Unlicense |
| **0b.** | Documentation | We will provide both **inline documentation** of the code and a basic **tutorial** that explains how a user can (for example) spin up test net and run contracts with transactions,
| **0c.** | Testing and Testing Guide | Core functions will be fully covered by comprehensive unit tests to ensure functionality and robustness. In the guide, we will describe how to run these tests. |
| **0d.** | Docker | We will provide a Dockerfile(s) that can be used to test all the functionality delivered with this milestone. |
| 0e. | Article | We will publish an **article**/workshop that explains [...] (what was done/achieved as part of the grant). (Content, language and medium should reflect your target audience described above.) |
| 1. | updated evm contracts | We will update tool with support for newest Moonbeam parachain|
| 2. | launch scripts | Scripts which will allow to launch the tool on test net|
| 3. | support for solidity-wasm contracts | We will deliver support for solidity contract compiled with solang to wasm|


### Milestone 2 Example — Additional features

- **Estimated Duration:** 1 month
- **FTE:**  0,5
- **Costs:**

| Number | Deliverable | Specification |
| -----: | ----------- | ------------- |
| 1. | CI integration | Tool integrated with CI for Moonbeam releses |
| 2. | data collector| messurement data collector library sending them to DB |
| 3. | visual dashboard | Visual dashboard with performance plots |


...


## Future Plans

We are going to promote the project writting articles and involve other developers to maintain it in the future

## Referral Program (optional) :moneybag: 

You can find more information about the program [here](../README.md#moneybag-referral-program).
- **Referrer:** Name of the Polkadot Ambassador or GitHub account of the Web3 Foundation grantee
- **Payment Address:** BTC, Ethereum (USDC/DAI) or Polkadot/Kusama (USDT) payment address. Please also specify the currency. (e.g. 0x8920... (DAI))

## Additional Information :heavy_plus_sign:

**How did you hear about the Grants Program?** Parity team

