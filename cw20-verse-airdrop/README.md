# CW20 VERSE Airdrop

This is a merkle airdrop smart contract that makes cw20 token distributions cheap and efficient.

## Spec

### Messages

#### InstantiateMsg

`InstantiateMsg` instantiates contract with owner and cw20 token address. Airdrop `stage` is set to 0.

```rust
pub struct InstantiateMsg {
  pub owner: String,
  pub cw20_token_address: String,
}
```

#### ExecuteMsg

```rust
pub enum ExecuteMsg {
  UpdateConfig {
    owner: Option<String>,
  },
  RegisterMerkleRoot {
    merkle_root: String,
  },
  Claim {
    stage: u8,
    amount: Uint128,
    proof: Vec<String>,
  },
}
```

- `UpdateConfig{owner}` updates configuration.
- `RegisterMerkleRoot {merkle_root}` registers merkle tree root for further claim verification. Airdrop `Stage`
  increased by 1.
- `Claim{stage, amount, proof}` recipient executes for claiming airdrop with `stage`, `amount` and `proof` data built
  using full list.

#### QueryMsg

``` rust
pub enum QueryMsg {
    Config {},
    MerkleRoot { stage: u8 },
    LatestStage {},
    IsClaimed { stage: u8, address: String },
}
```

- `{ config: {} }` returns configuration, `{"cw20_token_address": ..., "owner": ...}`.
- `{ merkle_root: { stage: "1" }` returns merkle root of given stage, `{"merkle_root": ... , "stage": ...}`
- `{ latest_stage: {}}` returns current airdrop stage, `{"latest_stage": ...}`
- `{ is_claimed: {stage: "stage", address: "wasm1..."}` returns if address claimed airdrop, `{"is_claimed": "true"}`