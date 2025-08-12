### AMM-Specific Reload Triggers
Reload token accounts or mints in AMM instructions if:

 Providing liquidity — need current vault balances before minting LP tokens.

 Removing liquidity — need current LP supply, vault balances, and user LP token balance.

 Swapping — need fresh reserves to calculate output amount.

 Fee distribution — depends on updated balances before/after fees.

 Rebalancing or sync — require accurate reserves to avoid skewed pricing.
