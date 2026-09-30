# G2 MFL — CHIP LEDGER LAW (ONE MINT)
**Date:** 2026-09-30
**Issuer:** Ghost
**Status:** LOCKED FOR SANDBOX ENGINEERING

## Law
1. One economic rail: $MFL.
2. Chips are a skin. They are not a mint, not an ATA, not a second balance column.
3. Every write uses `base_units` (Token-2022 decimals = 6).
4. Fast-5 and poker read the same currency field.
5. No SOL in any pot.
6. No wrap, no booth, no city-coin chip conversion.
7. Sandbox asset id = `MFL_TEST`. Live asset id is not armed here.
8. Test balances are non-transferable. UI may look like a stack. Chain send stays off.

## Display
`chips = floor(base_units / chip_scale)`
Default `chip_scale = 10000` (0.01 MFL per chip) unless a table overrides.
Rounding lives only in the renderer.

## Drift test
After every hand:
`sum(seat.stack_base) + table.pot_base + table.rake_base == table.session_in_base`
Fail this test = do not UNLOCK.

OVER.
