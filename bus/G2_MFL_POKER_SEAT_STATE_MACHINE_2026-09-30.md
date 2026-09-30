# G2 MFL — POKER SEAT STATE MACHINE (SANDBOX)
**Date:** 2026-09-30
**Issuer:** Ghost
**Game:** 5-card draw first (Hold’em later)
**Asset:** single $MFL mint · chips = display units only
**Charge:** TEST ONLY · non-transferable test balances · no live Token-2022 move

## 1. Purpose
Give Fast-5 and poker the same economic rail so accounting cannot drift.

Fast-5 already: lock → pot → settle → same $MFL.
Poker sits on that rail with extra streets.

## 2. Canonical flow
```
WALLET → SIT → LOCK_MFL → DEAL → BET → DRAW → FINAL_BET → SHOWDOWN → SETTLE → UNLOCK
```

Illegal to skip LOCK_MFL before DEAL.
Illegal to SETTLE before SHOWDOWN (except FOLD_WIN or TIMEOUT_REFUND).
Illegal to UNLOCK without a settle record.

## 3. States

| State | Who can act | What is locked | Next legal events |
|---|---|---|---|
| WALLET | player | nothing | CONNECT, SIT_REQUEST |
| SIT | table | reserved seat, no funds | LOCK_REQUEST, LEAVE |
| LOCK_MFL | escrow sim | buy-in in test $MFL | LOCK_OK, LOCK_FAIL, LEAVE |
| DEAL | engine | buy-in | HOLE_DEALT |
| BET | seats in hand | buy-in + street bets | CHECK, BET, CALL, RAISE, FOLD, ALLIN |
| DRAW | remaining seats | pot | DISCARD_0_5, STAND_PAT |
| FINAL_BET | remaining seats | pot | CHECK, BET, CALL, RAISE, FOLD, ALLIN |
| SHOWDOWN | engine | pot | RANK_HANDS |
| SETTLE | ledger | pot | PAY_WINNER, SPLIT_POT, RAKE_PROTOCOL, REFUND |
| UNLOCK | escrow sim | none after write | RETURN_STACK, SIT_OUT, REBUY |

Abort paths:
- LOCK_FAIL → WALLET (no pot)
- LEAVE before LOCK_OK → WALLET
- FOLD until one seat remains → SETTLE (FOLD_WIN) → UNLOCK
- ACTION_TIMEOUT → FOLD (or TIMEOUT_REFUND if table dead / engine mute)

## 4. Money law (do not invent a chip asset)
- Denomination: raw $MFL base units. 6 decimals. `1 MFL = 1_000_000`.
- UI may show chips. `chip_display = floor(base_units / chip_scale)`.
- `chip_scale` is cosmetic (example 10_000 base = 1 chip). Never stored as a second balance.
- Buy-in, blinds/antes, bets, rake, payouts: all base units.
- Fast-5 pots and poker pots share the same ledger currency field: `asset = MFL_TEST` while sandbox.

## 5. Table stakes (sandbox numbers, not live tickets)
Display ladder only. Engine stores base units.

| Seat tier | Display buy-in | Blind/ante sketch |
|---|---|---|
| Rookie | 10 MFL | ante 0.2 |
| Regular | 50 MFL | ante 1 |
| Prime | 250 MFL | ante 5 |

No mixed bags. No city-coin buy-in. City coins stay flavor / Fast-5 chemistry, not poker chips.

## 6. Settlement sketch (sandbox)
Winner takes pot minus protocol rake.
Suggested sandbox rake: 5% protocol, same family as Fast-5 H2H test split.
Ties: split remainder; rake 0 on exact tie (match Fast-5 refund-on-tie spirit).
Timeout / engine mute: 100% refund to stacks. No silent confiscation.

Do not enable mainnet transfer in this unit.

## 7. Seat object (minimum fields)
```
seat_id
room_id
handle            # public identity; not raw wallet dump
wallet_ref        # hashed / internal
state
buy_in_base
stack_base
committed_base
hole_cards[]      # server-only until showdown
discards[]
hand_rank
last_action_at
```

Table object:
```
table_id
variant           # FIVE_CARD_DRAW
max_seats
button_seat
pot_base
rake_bps
chip_scale        # UI only
asset             # MFL_TEST
live_transfer     # false
```

## 8. Event log (audit)
Append-only. One line per event.
Required fields: `ts, table_id, seat_id, event, delta_base, pot_base_after, stack_base_after, state_from, state_to`.
Never log a chip integer without the matching `delta_base`.

## 9. UI contract (mobile)
- One mint badge. No swap widget.
- Stack shown as chips with a small `$MFL` sublabel.
- Lock button copies Fast-5 locker language: LOCK / NO LIVE TOKEN CHARGE while sandbox.
- Streets labeled DEAL / BET / DRAW / FINAL / SHOW.
- After UNLOCK, player can SIT again or leave to Fast-5.

## 10. Out of scope this unit
- Hold’em community cards
- Real Token-2022 CPI
- Paymaster / EOA custody
- Football scoring engine
- Public payout tables on X

## 11. Acceptance
- State machine cannot deal before lock.
- Ledger sums: sum(stacks) + pot + rake_bucket = sum(buy_ins) at every event.
- Chip display can be wrong by rounding; base units cannot.
- Flipping `live_transfer=true` is a separate chair order, not this packet.

OVER.
