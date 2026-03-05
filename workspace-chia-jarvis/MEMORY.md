# MEMORY.md - Chia Programmer Learnings from Rigidity Repos

## Rigidity Style Guide

Studied Rigidity's GitHub repos (cloned 15 key Chia-related: auction-clsp, cat-tool, chia_rs, etc.). Focus on Chialisp code in auction-clsp and option-contract-clsp, plus Rust tooling patterns.

### Chialisp Best Practices & Patterns
1. **Modular Decomposition**:
   - Split puzzles into focused files: main auction.clsp, distribution, bid_programs/{initial,linear,static}, include/auction_truths.clsp.
   - Use `(include ...)` for shared utilities (auction_truths.clsp, curry.clib, utility_macros.clib).

2. **Currying & Mod Arguments**:
   - Pre-curry fixed params into mod (e.g., `(mod (BID_PROGRAM SELF_HASH)`).
   - Enables composability; referenced in comments.

3. **Inline Functions**:
   - `(defun-inline bid_index (bid) (f bid))` for simple cons accessors.
   - `(defun-inline bid_amount (bid) (r bid))`.

4. **Opcode Constants**:
   - `(defconstant CREATE_COIN 51)` (51), `(defconstant SEND_MESSAGE 66)`.

5. **Recursive List Processing**:
   - Condition morphing: `morph_conditions` recursively wraps CREATE_COIN (51) in option layer.
   - Handles melting/exercising options, asserts balance.

6. **Validation & Errors**:
   - Strict `if` chains, `assert` for invariants (e.g., melted iff exercised).
   - Return `(x)` on failure (softfail), `()` on success? Wait, standard puzzle: () fail, list success.

7. **Naming Conventions**:
   - snake_case: `construct_bid_truths`, `inner_puzzle_hash`, `wrap_puzzle_hash`.
   - Descriptive args.

8. **Advanced Utilities**:
   - `curry_hashes` for puzzle hashing.
   - `wrap_create_coin`, `morph_conditions` for layered puzzles.

### Rust/Chia Tooling Patterns
- **SDK Usage**: cat-tool uses Wallet SDK for CAT issue/melt; connects to fullnode peer.
- **Consensus**: Forks/maintains chia_rs for bls, consensus, puzzles.
- **Testing**: Emphasize testnet11; disclaimers for mainnet.
- **CLI**: Cargo install, clap args (e.g., `--amount 1 --uri localhost:58444`).
- **Fuzzing/Tests**: chia_rs has fuzzers, benchmarks, pre-commit hooks (fmt, clippy).

### Key Repos Analyzed
- **auction-clsp**: Auction house with pluggable bid programs (initial, linear increase, static).
- **option-contract-clsp**: Option wrapper puzzle; morphs underlying conditions.
- **cat-tool**: CAT admin CLI.
- **chia_rs**: Rust consensus/primitives.
- Others: puzzle-parser, mempool-inserter, thyme (coin cache), xch-chat.

Learnings integrated: Future code will emulate this clean, modular, validated style.