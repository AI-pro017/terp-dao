# Terp DAO Contracts

CosmWasm smart contracts for Terp DAO: on-chain governance plus a set of NFT contracts for minting, staking and escrow.

The governance side is built on the DAO DAO v1 contracts by Zeke Medley. The NFT contracts started from the standard CosmWasm cw721 and escrow templates and were extended for this project.

## Contracts

| Contract | What it does |
| --- | --- |
| `contracts/cw-core` | The DAO itself. It holds the treasury, keeps track of its voting and proposal modules and executes passed proposals. |
| `contracts/cw-proposal-single` | Yes or no proposals with configurable quorum, threshold and voting period. |
| `contracts/customized_nft` | A cw721 NFT contract with extra collection details (name, description, logo and banner) and a burn message. The `mint_number_limit` and `update_minter` options are in the messages but don't take effect yet. |
| `contracts/nft_staking` | Lets people stake NFTs from whitelisted collections. The admin creates a reward pool per collection with a reward per block and an optional end time, and stakers earn rewards for as long as their NFT stays staked. |
| `contracts/nft_auction` | For now, an escrow contract. It holds native or CW20 funds until an arbiter approves the release, or refunds them after a timeout. It's the base the auction flow is meant to grow from. |

Shared code lives in `packages/` (voting helpers, proposal and vote hooks, pagination and test utilities). `debug/` has small contracts that are only used in tests, such as a sudo proposal module and a simple CW20 balance voting module.

## Building

You'll need Rust with the `wasm32-unknown-unknown` target.

```bash
git clone https://github.com/AI-pro017/terp-dao.git
cd terp-dao
cargo test
```

To build optimized `.wasm` files for every contract in the workspace:

```bash
docker run --rm -v "$(pwd)":/code \
  --mount type=volume,source="$(basename "$(pwd)")_cache",target=/code/target \
  --mount type=volume,source=registry_cache,target=/usr/local/cargo/registry \
  cosmwasm/workspace-optimizer:0.12.6
```

The output goes to `artifacts/`.

## Related repos

Standalone versions of two of these contracts:

- [customized-nft](https://github.com/AI-pro017/customized-nft)
- [nft-staking](https://github.com/AI-pro017/nft-staking)
