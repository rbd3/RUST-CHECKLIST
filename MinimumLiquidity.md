### 1. Design Requirements


## Define Minimum Liquidity Amount:

Specify a constant for minimum LP tokens (e.g., const MINIMUM_LIQUIDITY: u64 = 1000) or minimum reserves (e.g., 1000 units of each token).



Ensure the amount is small enough to avoid significant user impact but large enough to prevent precision issues (e.g., 1000 LP tokens, as in Uniswap V2).



Example: In initialize_pool, reserve MINIMUM_LIQUIDITY LP tokens to a dead address.



### Choose Lock Mechanism:

**Option 1:** Mint minimum LP tokens to a dead address (e.g., Pubkey::default()) or protocol-controlled account during pool initialization.



**Option 2:** Enforce minimum reserves in remove_liquidity (e.g., require!(reserve_a - amount_a >= MINIMUM_RESERVE, AmmError::MinimumLiquidity)).



### Recommendation: Use LP token lock for simplicity, as it aligns with Uniswap and ensures proportional reserves.