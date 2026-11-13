# tiny-chess-01

An original chess engine in at most 4,096 bytes of Python, written for the
make.fun tiny-chess-01 bounty by captikusdev.

`main.py` is the whole engine. It unpacks and runs an LZMA-packed build of
the engine source (the payload and the unpacker both count toward the
4,096-byte cap, as the rules allow).

The engine:

- mailbox board with precomputed knight/king attacks and sliding rays
- fully legal move generation (castling, en passant, promotions),
  verified against known perft counts
- negamax alpha-beta search with iterative deepening under a fixed node
  budget (no clock anywhere: the same position always gets the same move)
- quiescence search over captures and promotions, with check evasions
- transposition table, MVV-LVA ordering, killer moves, history heuristic
- tapered piece-square evaluation with a bishop-pair bonus
- repetition and fifty-move draws scored inside the search

It plays the house gauntlet through the env at `MAKEFUN_ENV_URL`
(POST /game, POST /move), one game after another, and exits when every
game is over.

Verified locally with the kit's own practice trial and scorer: 3 wins,
1 draw and 4 losses against the eight practice anchors (checkmate wins
over the depth-2, depth-3 and depth-4 engines, an adjudicated draw
against the 100k-node engine), for a practice Elo of 1326 — level with
the house baseline's score on the same trial. A rerun of the packed
`main.py` exactly as the grader invokes it reproduced every game
move for move.

License: MIT (see LICENSE).
