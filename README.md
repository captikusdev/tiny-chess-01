# tiny-chess-01

An original chess engine in at most 4,096 bytes of Python, written for the
make.fun tiny-chess-01 bounty by captikusdev.

`main.py` is the whole engine. It unpacks and runs an LZMA-packed build of
the engine source (the payload and the unpacker both count toward the
4,096-byte cap, as the rules allow).

The engine:

- 0x88 board with precomputed knight/king attacks and sliding rays
- move generation (castling, en passant, promotions) verified against
  known perft counts; king safety is enforced by attack tests inside
  the search, so an illegal move is never returned
- negamax alpha-beta search with iterative deepening under a fixed
  90,000-node budget (no clock anywhere: the same position always gets
  the same move)
- quiescence search over captures and promotions, with check evasions
- transposition table, MVV-LVA capture ordering, history heuristic
- tapered piece-square evaluation with a bishop-pair bonus
- repetition and fifty-move draws scored inside the search

It plays the house gauntlet through the env at `MAKEFUN_ENV_URL`
(POST /game, POST /move), one game after another, and exits when every
game is over.

Verified locally with the kit's own practice trial and scorer: 2 wins,
2 draws and 4 losses against the eight practice anchors (checkmate wins
over the depth-2 and depth-4 engines, a stalemate draw against depth-3
and a threefold-repetition draw against the 100k-node engine), for a
practice Elo of 1253 — above the 1154 practice floor for the final.

License: MIT (see LICENSE).
