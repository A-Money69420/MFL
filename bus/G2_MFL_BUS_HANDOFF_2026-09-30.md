# G2 MFL BUS HANDOFF — POKER SEAT + ONE-MINT CHIPS
**Date:** 2026-09-30 18:40 EDT
**Surface:** Ghost
**Status:** SANDBOX LOCK · LIVE TRANSFER OFF · ONE MINT ONLY
**Parent Drive:** G2 (`1BvQ3KgQnamPQ_2ImBSB-RzfITJCC1AG4`)
**Public locker:** https://github.com/A-Money69420/MFL (repo is file locker, not live board)

## Why this packet exists
Stop ferrying design in chat. Next engineering unit is the poker seat-state machine on the same $MFL rail as Fast-5.

## Files in this drop
| File | Job |
|---|---|
| `G2_MFL_POKER_SEAT_STATE_MACHINE_2026-09-30.md` | Human spec |
| `mfl_poker_seat_machine_v1.json` | Machine-readable states + events |
| `G2_MFL_CHIP_LEDGER_ONE_MINT_2026-09-30.md` | Ledger law: chips are display only |

## Standing gates (do not relax in this packet)
- One mint. No second token. No SOL pot. No conversion booth.
- Ledger always in raw $MFL base units (6 decimals). Chip counts are UI paint.
- Live token charge remains blocked. Test balances only until legal/compliance + escrow audit clear.
- Football scoring math stays off public X / public pages. This packet does not reprint it.
- Kitchen = private loop. Telegram/Discord = hangout, not intake.

## Seat machine (canonical)
`WALLET → SIT → LOCK_MFL → DEAL → BET → DRAW → FINAL_BET → SHOWDOWN → SETTLE → UNLOCK`

## Pickup
1. Drive G2 (this folder / paper copies).
2. GitHub `A-Money69420/MFL` path `bus/`.
3. Chat artifacts rendered below.

OVER.
