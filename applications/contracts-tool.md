# Contracts performance messurement tool

- **Gloslab:**
- **Payment Address:** Ethereum (USDC/DAI)
- **[Level](https://github.com/w3f/Grants-Program/tree/master#level_slider-levels):** 1, 2 or 3

## Project Overview :page_facing_up:

### Overview

The key objective of this grant is to build a tool which allows to compare contracts performance and to track its regressions.
Subject of comparision are solidity, and ink! contracts running on parachains (pallet-contract and pallet-evm).
Tool will be integrated with CI and running each night, generating messurements data and providing its visualisation.
Our team has strong interest in contracts development on Polkadot and building Polkadot ecosystem.

### Project Details

#### Generating benchmarking results

Project is based on smart-bench Parity repo https://github.com/paritytech/smart-bench and developed in rust language.
Apart of existing ink! and solidity contracts support, will be introduces support for solang compiled contract running on pallet-contract.
Finally the tool will messure performance of:
- Ink! contract run on pallet-contract (currently supported)
- Solidity contracts comiled with solc run on pallet-evm (supported but outdated)
- Solidity contracts compiled with solang and run on pallet-contract (new funtionality)

The tool is run with following commands:
```
smart-bench [OPTIONS] --instance-count <INSTANCE_COUNT> --call-count <CALL_COUNT> <CHAIN> [CONTRACTS] --url <WS_NODE_ADRESS>
```
where as a chain can be used ink-wasm, sol-wasm or evm

example:
```
cargo run --release -- ink-wasm erc20 erc1155 --instance-count 10 --call-count 20 --url ws://localhost:9988
cargo run --release -- sol-wasm erc20 erc1155 --instance-count 10 --call-count 20 --url ws://localhost:9988
cargo run --release -- evm erc20 erc1155 --instance-count 10 --call-count 20 --url ws://localhost:9988
```

The performance messurements are run against test network, which is setup using zombinet.
Required scripts are delivered with the tool.

The tool is integrated with github CI, and each night produce messurements data for contracts 
using newest released versions of parachain.
Messurement data are sent by collector to database for further processing.
Each collected stats contains:
```
Block number
PoV size 
Block Weight - reference time and proof size
Witness
Block size
Number of extrinsics processed in block
```

#### Performance tracking for CI/CD

Architecture for performance tracking tooling is built upon the concept of [Flat Data](https://githubnext.com/projects/flat-data), whereas sets of data is stored within repository itself. Data is being created and processed for storage on a timely schedule or it can be triggered by external event (pull request or merge event from other repositories) so the data in the repository can stay up to date.

To implement such architecture Github repository is created. It is to be self sufficient in terms of spinning up benchmarking environemnt, storing and processing benchmark results created within run of Github Action workflow. Repository contains all configuration files required for provisioning of benchmarking environemnt. Other provided utilities also allow to effortlessly start local Grafana and InfluxDB instances (available as Dockerfiles and Docker Compose configuration) for out of the box experience of running visualisations against data where all of this is part of the same repository. For syncing of most recent data standard git operations apply (sync local git repository with remote origin to get latest data). 

Github Action is responsible for running smart-bench software against zombienet to create benchmarking results. Results are then postprocessed to also include various metadata about environment used for its creation. Results are then commited and pushed to the repository. Metadata of benchmark results consists of various properties such as (consider following as draft, to be defined exactly as implementation proceeds):

- commit hashes of zombienet, parachain implementation (moonbeam or contract-pallet based) and smart-bench
- human readable versions of above if can be determined
- type of contract
- contract compiler
- parachain to run the contract

Measurements themselves are raw data as returned by smart-bench software. 

Performance tracking is concered with https://github.com/PureStake/moonbeam and https://github.com/paritytech/substrate (pallet-contracts) repositories . Coverage of the benchmarks strives to create results for all commits that are referenced by branches and pull requests of the repositories above (refs/heads/* and refs/pull/*/merge). So for every branch and pull request, at the time of running of Github Action's workflow, results will be created and stored in the repository (exclusions possible to ignore old/EOL branches). Github Action's workflow determines non-existing benchamrking results per commit hash and makes use of matrix strategy to concurently run jobs in parallel for all missing hashes.

Summary of items provided by repository:
- Dockerfiles to run grafana, influxdb, smart-bench
- Docker Compose to ease local setup of all components
- Scripts to transform smart-bench output to data format ingestible by InfluxDB
- CLI app to implement common tasks to work with project:
    - find measurements for given set of metadata values / pull request / commit hash
    - request to create measurements for given set of metadata values / pull request / pull request / commit hash 
    - check status of measurements creation process
    - determine branches and pull requests for which benchmarking results are missing
- Scripts implementing uploading data to InfluxDB will try to parse benchmarking data in smart-bench outputed format to extract separate measurements and accompany them with metadata to create contexts for visualisations. Repository will also provide configuration files for dashboards of Grafana

Limitations:Investigate 
the project does not compile the contracts by itself, contracts are delivered in binary form.


### Ecosystem Fit

The project is usefull for ecosystem at contracts development stage to track its performance and regressions on Polkadot.
It is going to be used also to messure ink! language performance by Parity core team.

## Team :busts_in_silhouette:

### Team members

- Sebastian Miasojed
- Karol Kokoszka

### Contact

- **Contact Name:** Sebastian Miasojed
- **Contact Email:** s.miasojed@gmail.com
- **Website:** 

- **Contact Name:** Karol Kokoszka
- **Contact Email:** karol.k91@gmail.com
- **Website:** 

### Legal Structure

- **Registered Address:** Address of your registered legal entity, if available. Please keep it in a single line. (e.g. High Street 1, London LK1 234, UK)
- **Registered Legal Entity:** Name of your registered legal entity, if available. (e.g. Duo Ltd.)

### Team's experience

We combine development and architecting skills from embdded world, cloud systems and apply them to crypto world.
Until now the team has shown his profficiency alligning smart-bench with newest libraries required by ink! contracts.

### Team Code Repos

- https://github.com/smiasojed
- https://github.com/karolk91

### Team LinkedIn Profiles (if available)

- https://www.linkedin.com/in/sebastian-miasojed-83b6123
- https://www.linkedin.com/in/karol-kokoszka-a66959103


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
| **0a.** | License | GPLv3 |
| **0b.** | Documentation | We will provide both **inline documentation** of the code and a basic **tutorial** that explains how a user can (for example) spin up test net and run contracts with transactions,
| **0c.** | Testing and Testing Guide | Core functions will be fully covered by comprehensive unit tests to ensure functionality and robustness. In the guide, we will describe how to run these tests. |
| **0d.** | Docker | We will provide a Dockerfile(s) that can be used to test all the functionality delivered with this milestone. |
| 0e. | Article | We will publish article on Medium that explains what was done/achieved as part of the grant|
| 1. | updated evm contracts | We will update tool with support for newest Moonbeam parachain|
| 2. | launch scripts | Scripts which will allow to launch the tool on test net|
| 3. | support for solidity-wasm contracts | We will deliver support for solidity contract compiled with solang to wasm|


### Milestone 2 Example — Additional features

- **Estimated Duration:** 1 month
- **FTE:**  0,5
- **Costs:**

| Number | Deliverable | Specification |
| -----: | ----------- | ------------- |
| 1. | Dockerized benchmarking | Create Docker and docker-compose related configurations to build and start smart-bench, zombienet and parachains to generate benchmarking results |
| 2. | Github Actions benchmark jobs | Create workflow and implement a job to utilize Dockerized benchmarking for generating results and uploading them to repostory |
| 3. | Results processing tools | Implementation of tooling to translate smart-bench output format to format of InfluxDB |
| 4. | Dockerized visualisations | Create Docker and docker-compose related configurations to run Grafana and InfluxDB preconfigured with dashboards and measurements |
| 5. | CLI tool | Implementation of tooling to determine branches and pull requests of remote repositories for which measurements are missing |
| 6. | Github Actions workflow | Create complete workflow running parallel jobs based on matrix strategy for all missing measurements |

...


## Future Plans

We are going to promote the project writting article and involve other developers to maintain it in the future

## Referral Program (optional) :moneybag: 

You can find more information about the program [here](../README.md#moneybag-referral-program).
- **Referrer:** Name of the Polkadot Ambassador or GitHub account of the Web3 Foundation grantee
- **Payment Address:** BTC, Ethereum (USDC/DAI) or Polkadot/Kusama (USDT) payment address. Please also specify the currency. (e.g. 0x8920... (DAI))

## Additional Information :heavy_plus_sign:

**How did you hear about the Grants Program?** Parity team

