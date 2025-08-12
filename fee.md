### Define Fee Structure:


Confirm the LP fee is 0.3% (3/1000) of the output amount (amount_out), deducted before transfer to the user.

Example: For amount_out = 1000, fee = (1000 * 3) / 1000 = 3 tokens.

Prevent Zero Fees:

Ensure fees are non-zero for any non-zero amount_out to avoid bypassing fees for small swaps (amount_out < 333).

Choose one of two mitigations:

Ceiling Division: Use (amount_out as u128 * 3 + 999) / 1000 to round up fees.
Minimum Fee: Use std::cmp::max(1, (amount_out as u128 * 3).div_floor(&1000) as u64) to enforce a minimum fee of 1 token.



