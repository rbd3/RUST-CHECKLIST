Rust Audit Checklist
Integer Overflow & Underflow
Checklist Items

Use Checked Math Operations: Always use checked arithmetic methods (e.g., checked_add, checked_sub, checked_mul, checked_div) for integer operations to prevent overflow or underflow in release mode. Unchecked operations (e.g., +, -, *, /) may wrap around with two's complement in release mode, leading to unexpected behavior.
Validate Arithmetic Results: Ensure arithmetic operations are checked for overflow/underflow using methods like .ok_or() to return an error (e.g., ProgramError::InvalidArgument) if the operation fails.
Avoid Unchecked Casts: Do not use unchecked type conversions (e.g., as u32 on a u64 value), as they truncate values and can cause data loss or unexpected behavior. Use checked conversions like <type>::try_from(...) to handle conversions safely.
Test Edge Cases: Include tests for boundary values (e.g., u32::MAX, u64::MAX, negative values for signed types) to ensure the program handles extreme inputs correctly and prevents wraparound.
Check Solana BPF Context: When compiling with the Solana BPF toolchain (cargo build-bpf), ensure all arithmetic and type conversions are checked, as this toolchain operates in release mode, disabling default overflow checks.
Audit User Input Handling: Verify that user-provided inputs (e.g., amount in a withdrawal function) are validated to prevent exploitation through large values that could trigger overflow/underflow.
Document Assumptions: Clearly document any assumptions about integer ranges or expected values in the code to guide future audits and maintenance.

Example
Below is an example of safe arithmetic to prevent overflow in a token withdrawal function:
let FEE: u32 = 1000;

fn withdraw_token(program_id: &Pubkey, accounts: &[AccountInfo], amount: u32) -> ProgramResult {
    // ... deserialize & validate user and vault accounts ...

    if amount.checked_add(FEE).ok_or(ProgramError::InvalidArgument)? > vault.user_balance[user_id] {
        return Err(ProgramError::AttemptToWithdrawTooMuch);
    }

    // ... Transfer `amount` tokens from vault to user-controlled account ...

    Ok(())
}

This code uses checked_add to ensure the addition of amount and FEE does not overflow, returning an error if it does, thus preventing unauthorized withdrawals.