# indexer-service-rs

test

## Introduction

A Rust impl for The Graph [indexer service](https://github.com/graphprotocol/indexer/tree/main/packages/indexer-service) to provide data services as an Indexer, integrated with [TAP](https://github.com/semiotic-ai/timeline-aggregation-protocol) which is a fast, efficient, and trustless unidirectional micro-payments system.

## Features

- Receive paid or free query requests and route to graph node
- Route "meta" queries on indexing statuses and deployment health
- Serve indexer information such as health, indexer version, and operator address
- Monitor allocations, attestation signers, and manage receipts using TAP, store receipts in the indexer database
- Record performance and service metrics

## Quick start

Configure required start-up args, check description by 
```
cargo run -p service -- --help
```

Set up configurations. To run with toml configurations
```
cargo run -- config "template.toml"

```

To run with CLI args
```
cargo run -- --ethereum <eth-node-provider> \
  --mnemonic <operator-mnemonic> \
  --indexer-address  <indexer-address ></indexer-address> \
  --port 7300 \
  --metrics-port 7500 \
  --graph-node-query-endpoint http://localhost:8000 \
  --graph-node-status-endpoint http://localhost:8030 \ 
  --free-query-auth-token "free-query-auth" \
  --postgres-host "127.0.0.1" \
  --postgres-port 5432 \
  --postgres-database postgres  \
  --postgres-username <postgres-username> \
  --postgres-password <postgres-password> \
  --network-subgraph-endpoint "https://api.thegraph.com/subgraphs/name/graphprotocol/graph-network \
  --network-subgraph-auth-token "network-subgraph-auth" \
  --serve-network-subgraph true \
  --client-signer-address "0xe1EC4339019eC9628438F8755f847e3023e4ff9c" \

```


## Configuring 

```
✗ cargo run -p service -- --help

Usage: service [OPTIONS] <--ethereum <ethereum-node-provider>|--ethereum-polling-interval <ethereum-polling-interval>|--mnemonic <mnemonic>|--indexer-address <indexer-address>> <--port <port>|--metrics-port <metrics-port>|--graph-node-query-endpoint <graph-node-query-endpoint>|--graph-node-status-endpoint <graph-node-status-endpoint>|--log-level <log-level>|--gcloud-profiling|--free-query-auth-token <free-query-auth-token>> <--postgres-host <postgres-host>|--postgres-port <postgres-port>|--postgres-database <postgres-database>|--postgres-username <postgres-username>|--postgres-password <postgres-password>> <--network-subgraph-deployment <network-subgraph-deployment>|--network-subgraph-endpoint <network-subgraph-endpoint>|--network-subgraph-auth-token <network-subgraph-auth-token>|--serve-network-subgraph|--allocation-syncing-interval <allocation-syncing-interval>|--client-signer-address <client-signer-address>>

Options:
      --ethereum <ethereum-node-provider>
          Ethereum node or provider URL [env: ETH_NODE=]
      --ethereum-polling-interval <ethereum-polling-interval>
          Polling interval for the Ethereum provider (ms) [env: ETHEREUM_POLLING_INTERVAL=] [default: 4000]
      --mnemonic <mnemonic>
          Mnemonic for the operator wallet [env: MNEMONIC=]
      --indexer-address <indexer-address>
          Ethereum address of the indexer [env: INDEXER_ADDRESS=]
      --port <port>
          Port to serve queries at [env: PORT=] [default: 7600]
      --metrics-port <metrics-port>
          Port to serve Prometheus metrics at [env: METRICS_PORT=] [default: 7300]
      --graph-node-query-endpoint <graph-node-query-endpoint>
          Graph node GraphQL HTTP service endpoint [env: GRAPH_NODE_QUERY_ENDPOINT=] [default: http://0.0.0.0:8000]
      --graph-node-status-endpoint <graph-node-status-endpoint>
          Graph node endpoint for the index node server [env: GRAPH_NODE_STATUS_ENDPOINT=] [default: http://0.0.0.0:8030]
      --log-level <log-level>
          Log level in RUST_LOG format [env: LOG_LEVEL=]
      --gcloud-profiling
          Whether to enable Google Cloud profiling [env: GCLOUD_PROFILING=]
      --free-query-auth-token <free-query-auth-token>
          Auth token that clients can use to query for free [env: FREE_QUERY_AUTH_TOKEN=]
      --postgres-host <postgres-host>
          Postgres host [env: POSTGRES_HOST=] [default: http://0.0.0.0/]
      --postgres-port <postgres-port>
          Postgres port [env: POSTGRES_PORT=] [default: 5432]
      --postgres-database <postgres-database>
          Postgres database name [env: POSTGRES_DATABASE=]
      --postgres-username <postgres-username>
          Postgres username [env: POSTGRES_USERNAME=] [default: postgres]
      --postgres-password <postgres-password>
          Postgres password [env: POSTGRES_PASSWORD=] [default: ]
      --network-subgraph-deployment <network-subgraph-deployment>
          Network subgraph deployment [env: NETWORK_SUBGRAPH_DEPLOYMENT=]
      --network-subgraph-endpoint <network-subgraph-endpoint>
          Endpoint to query the network subgraph from [env: NETWORK_SUBGRAPH_ENDPOINT=] [default: https://api.thegraph.com/subgraphs/name/graphprotocol/graph-network-goerli]
      --network-subgraph-auth-token <network-subgraph-auth-token>
          Bearer token to require for /network queries [env: NETWORK_SUBGRAPH_AUTH_TOKEN=]
      --serve-network-subgraph
          Whether to serve the network subgraph at /network [env: SERVE_NETWORK_SUBGRAPH=]
      --allocation-syncing-interval <allocation-syncing-interval>
          Interval (in ms) for syncing indexer allocations from the network [env: ALLOCATION_SYNCING_INTERVAL=] [default: 120000]
      --client-signer-address <client-signer-address>
          Address that signs query fee receipts from a known client [env: CLIENT_SIGNER_ADDRESS=]
  -c <config>
          Indexer service configuration file (YAML format) [env: CONFIG=]
  -h, --help
          Print help
  -V, --version
          Print version
```
## Upgrading

We follow conventional semantics for package versioning. An indexer may set a minor version specification for automatic patch updates while preventing breaking changes. To safely upgrading the package, we recommend the following steps:

1. **Review Release Notes**: Before upgrading, check the release notes for the new version to understand what changes, fixes, or new features are included.
2. **Review Documentation**: Check the up-to-date documentation for an accurate reflection of the changes made during the upgrade.
3. **Backup Configuration**: Save your current configuration files and any local modifications you've made to the existing codebase.
4. **Deploy**: Replace the old executable or docker image with the new one and restart the service to apply the upgrade. 
5. **Monitor and Validate**: After the upgrade, monitor system behavior and performance metrics to validate that the service is running as expected.

These steps should ensure a smooth transition to the latest version of `indexer-service-rs`, harnessing new capabilities while maintaining system integrity.

## Contributing

[Contributions guide](/contributing.md)


### Supported request and response format examples


```
✗ curl http://localhost:7300/
Ready to roll! 

✗ curl http://localhost:7300/health
{"healthy":true}

✗ curl http://localhost:7300/version
{"version":"0.1.0","dependencies":{}}

✗ curl http://localhost:7300/operator/info
{"publicKey":"0xacb05407d78129b5717bb51712d3e23a78a10929"}

# Subgraph queries
# Checks for receipts and authorization
✗ curl -X POST -H 'Content-Type: application/json' -H 'Authorization: Bearer token-for-graph-node-query-endpoint' --data '{"query": "{_meta{block{number}}}"}' http://localhost:7300/subgraphs/id/QmacQnSgia4iDPWHpeY6aWxesRFdb8o5DKZUx96zZqEWrB
"{\"data\":{\"_meta\":{\"block\":{\"number\":9425787}}}}"

# Takes hex representation for subgraphs deployment id aside from IPFS hash representation
✗ curl -X POST -H 'Content-Type: application/json' -H 'Authorization: Bearer token-for-graph-node-query-endpoint' --data '{"query": "{_meta{block{number}}}"}' http://localhost:7300/subgraphs/id/0xb655ca6f49e73728a102219726ff678d61d8fb792874792e9f0d9887dc616600
"{\"data\":{\"_meta\":{\"block\":{\"number\":9425787}}}}"

# Free query auth token check failed
✗ curl -X POST -H 'Content-Type: application/json' -H 'Authorization: blah' --data '{"query": "{_meta{block{number}}}"}' http://localhost:7300/subgraphs/id/0xb655ca6f49e73728a102219726ff678d61d8fb792874792e9f0d9887dc616600
"Invalid Scalar-Receipt header provided"%

# Subgraph health check
✗ curl http://localhost:7300/subgraphs/health/QmVhiE4nax9i86UBnBmQCYDzvjWuwHShYh7aspGPQhU5Sj
"Subgraph deployment is up to date"%                    
## Unfound subgraph
✗ curl http://localhost:7300/subgraphs/health/QmacQnSgia4iDPWHpeY6aWxesRFdb8o5DKZUx96zZqEWrB
"Invalid indexing status"%   

# Network queries
# Checks for auth and configuration to serve-network-subgraph
✗ curl -X POST -H 'Content-Type: application/json' -H 'Authorization: token-for-network-subgraph' --data '{"query": "{_meta{block{number}}}"}' http://localhost:7300/network 
"Not enabled or authorized query"

# Indexing status resolver - Route supported root field queries to graph node status endpoint
✗ curl -X POST -H 'Content-Type: application/json' --data '{"query": "{blockHashFromNumber(network:\"goerli\", blockNumber: 9069120)}"}' http://localhost:7300/status 
{"data":{"blockHashFromNumber":"e1e5472636db73ba5496aee098dc21310683c95eb30fc46f9ba6c36d8b28d58e"}}%                

# Indexing status resolver - 
✗ curl -X POST -H 'Content-Type: application/json' --data '{"query": "{indexingStatuses {subgraph health} }"}' http://localhost:7300/status 
{"data":{"indexingStatuses":[{"subgraph":"QmVhiE4nax9i86UBnBmQCYDzvjWuwHShYh7aspGPQhU5Sj","health":"healthy"},{"subgraph":"QmWVtsWk8Pqn3zY3czDjyoVreshRLmoz9jko3mQ4uvxQDj","health":"healthy"},{"subgraph":"QmacQnSgia4iDPWHpeY6aWxesRFdb8o5DKZUx96zZqEWrB","health":"healthy"}]}}

# Indexing status resolver - Filter out the unsupported queries
✗ curl -X POST -H 'Content-Type: application/json' --data '{"query": "{_meta{block{number}}}"}' http://localhost:7300/status 
{"errors":[{"locations":[{"line":1,"column":2}],"message":"Type `Query` has no field `_meta`"}]}%              

######## Cost server - read-only graphql query
curl -X GET -H 'Content-Type: application/json' --data '{"query": "{ costModel(deployment: \"Qmb5Ysp5oCUXhLA8NmxmYKDAX2nCMnh7Vvb5uffb9n5vss\") { deployment model variables }} "}' http://localhost:7300/cost

curl -X GET -H 'Content-Type: application/json' --data '{"query": "{ costModel(deployment: \"Qmb5Ysp5oCUXhLA8NmxmYKDAX2nCMnh7Vvb5uffb9n5vss\") { deployment model variables }} "}' http://localhost:7300/cost
{"data":{"costModel":{"deployment":"0xbd499f7673ca32ef4a642207a8bebdd0fb03888cf2678b298438e3a1ae5206ea","model":"default => 0.00025;","variables":null}}}%

curl -X GET -H 'Content-Type: application/json' --data '{"query": "{ costModel(deployment: \"Qmb5Ysp5oCUXhLA8NmxmYKDAX2nCMnh7Vvb5uffb9n5vas\") { deployment model variables }} "}' http://localhost:7300/cost
{"data":{"costModel":null}}%

curl -X GET -H 'Content-Type: application/json' --data '{"query": "{ costModel(deployment: \"Qmb5Ysp5oCUXhLA8NmxmYKDAX2nCMnh7Vvb5uffb9n5vss\") { deployment odel variables }} "}' http://localhost:7300/cost 
{"errors":[{"message":"Cannot query field \"odel\" on type \"CostModel\". Did you mean \"model\"?","locations":[{"line":1,"column":88}]}]}%     

curl -X GET -H 'Content-Type: application/json' --data '{"query": "{ costModels(deployments: [\"Qmb5Ysp5oCUXhLA8NmxmYKDAX2nCMnh7Vvb5uffb9n5vss\"]) { deployment model variables }} "}' http://localhost:7300/cost
{"data":{"costModels":[{"deployment":"0xbd499f7673ca32ef4a642207a8bebdd0fb03888cf2678b298438e3a1ae5206ea","model":"default => 0.00025;","variables":null}]}}%

```

## Dependency choices

- switching from actix-web to `axum` for the service server
- App profiling should utilize `perf`, flamegraphs or cpu profilers, and benches to track and collect performance data. The typescript implementation uses `gcloud-profile`
- Consider replacing and adding parts from TAP manager
- `postgres` database connection required to indexer management server database, shared with the indexer agent
- No migration in indexer service as it might introduce conflicts to the database; indexer agent is solely responsible for database management.


### Indexer common components

Temporarily live inside the indexer-service package under `src/common`.

Simple indexer management client to track NetworkSubgraph and postgres connection.
- NetworkSubgraph instance track both remote API endpoint and local deployment query endpoint. 
  - TODO: query indexing status of local deployment, only use remote API as fallback.
- Keeps cost model schema and resolvers with postgres and graphQL types: `costModel(deployment)` and `costModels(deployments)`. If deployments is empty, all cost models are returned.
  - Global cost model fallback used when specific deployments are queried
- No database migration in indexer service as it might introduce schema conflicts; indexer agent is solely responsible for database management.

### Indexer native dependency

Linked dependency could not be linked directly with git url "https://github.com/graphprotocol/indexer" and path "packages/indexer-native/native" at the same time, and could not access it on crates.io. So copid the folder to local repo with the version at https://github.com/graphprotocol/indexer/blob/972658b3ce8c512ad7b4dc575d29cd9d5377e3fe/packages/indexer-native/native.

Since indexer-service will be written in Rust and no need for typescript, indexer-native's neon build and util has been removed. 

Component `NativeSignatureVerifier` renamed to `SignatureVerifier`.

Separate package in the workspace under 'native'.

### common-ts components

Temporarily live inside the indexer-service package under `src/types`

- Address
- readNumber

## Components checklist (basic, not extensive)

- [x] Server path routing
  - [x] basic structure
  - [x] CORS
  - [x] timeouts
  - [x] Rate limiting levels
  - [x] Logger stream
- [ ] Query processor
  - [x] graph node query endpoint at specific subgraph path
  - [x] wrap request to and response from graph node
  - [x] extract receipt header
  - [x] Free query
    - [x] Query struct
    - [x] Free query auth token check
    - [x] Query routes + responses
    - [x] set `graph-attestable` in response header to `true`
  - [x] Network subgraph query
    - [x] Query struct
    - [x] serve network subgraph boolean + auth token check
    - [x] Query routes + responses
    - [x] set `graph-attestable` in response header to `false`
  - [ ] Paid query
    - [ ] receipts graphQL schema
    - [ ] [TAP](https://github.com/semiotic-ai/timeline-aggregation-protocol/) manager to handle receipts logic
      - [ ] derive, cache, and look up attestation signers
        - [ ] contracts - connect by network chain id
          - [ ] network provider
      - [x] validate receipt format (need unit tests)
      - [x] parse receipt (need unit tests)
      - [x] validate signature (need unit tests)
      - [ ] store
    - [ ] extract graph-attestable from graph node response header
    - [ ] monitor eligible allocations
      - [ ] network subgraph
      - [ ] operator wallet -> indexer address
  - [ ] subgraph health check
  - [ ] query timing logs
- [x] Deployment health server
  - [x] query status endpoint and process result
- [ ] Status server 
  - [x] indexing status resolver - to query indexingStatuses
  - [ ] Filter for unsupported queries
- [x] Cost server
  - [x] Simple indexer management client to track postgres connection and network subgraph endpoint.
  - [x] serve queries with defined graphQL schema and psql resolvers to database: `costModel(deployment)` and `costModels(deployments)`. If deployments is empty, all cost models are returned.
  - [x] Global cost model fallback used when specific deployments are queried
- [x] Constant service paths
  - [x] health
  - [x] ready to roll
  - [x] versions
  - [x] operator public key
    - [x] validate mnemonics to public key
- [x] Import indexer native
- [ ] Metrics
  - [x] Metrics setup
  - [x] serve basic indexer service metrics
  - [ ] Add cost model metrics 
- [x] CLI args
- [ ] App profiling
  - [ ] No gcloud profiling, can use `perf` to collect performance data.
