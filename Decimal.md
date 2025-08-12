## AMM Decimal Validation Checklist
1. During Pool Creation (initialize_pool)
 Check both tokens’ decimals:

Ensure token decimals are within an acceptable range (e.g., 1 ≤ decimals ≤ 18 for ERC-20 style tokens, ≤ 9 for SPL tokens).

Reject tokens with 0 decimals unless explicitly supported.

Reject tokens with extreme values (e.g., >18) to avoid precision loss.

 Check compatibility of token decimals:

If the AMM math assumes equal decimals, verify both tokens have the same decimal count.

If different decimals are allowed, ensure your math correctly handles scaling between them.

 Prevent non-standard tokens:

Ensure tokens follow expected SPL/ERC standards (e.g., fixed decimals, no dynamic decimal changes).

2. During Liquidity Addition / Removal
 Scale token amounts using decimals before adding to pool reserves.

 Verify that provided amounts are adjusted to match internal calculation units.

3. During Swaps
 Ensure swap math accounts for decimal differences:

Convert amounts to a common precision before ratio and price calculations.

 Prevent underflow/overflow in scaled amounts by validating decimal multiplications/divisions.

4. Special Cases
 If protocol supports stablecoins, allow fewer decimals (e.g., 6) but validate math precision.

 If protocol supports wrapped tokens (e.g., wBTC with 8 decimals), verify that swap formula doesn’t cause rounding errors.

 For governance tokens or experimental tokens, verify they aren’t using exotic decimal values to manipulate pool prices.