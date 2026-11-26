# Gambit — Play Chess

A polished, dependency-free chess app. Three ways to play:

- **vs Computer** — pick White, Black or Random, and four difficulty levels (Casual → Master).
- **Two Players** — local hot-seat play with optional auto-flipping board.
- **Spectate** — two engines battle it out, with a move-pace slider, pause and step controls.

Everything runs in the browser. No build step, no backend, no API keys.

## Features

- Complete chess rules: castling, en passant, promotion (with picker), check, checkmate, stalemate, threefold repetition, fifty-move rule and insufficient material.
- Drag-and-drop or click-to-move pieces, with smooth movement animations.
- Alpha-beta search engine with quiescence search and piece-square evaluation, running in a Web Worker so the UI never blocks.
- Live evaluation bar, captured-piece trays, material score and full move list in algebraic notation.
- One-click PGN copy, undo, board flip, resign and new game.
- Subtle synthesized sound effects (toggle, remembered between visits).
- Responsive layout, keyboard shortcuts (`F` flip, `U` undo, `Esc` close).

## Deploy

The site is fully static — upload the folder as-is.

**Netlify** — drag the `gambit-chess` folder onto <https://app.netlify.com/drop>.

**Vercel** — `npx vercel` from this folder, accept the defaults (no framework, no build command).

**GitHub Pages** — push the folder to a repository, then enable Pages for the branch root.

**Cloudflare Pages / Surge / any static host** — point it at this folder.

To preview locally, any static server works:

```bash
npx serve .
# or
python -m http.server 8000
```

> Opening `index.html` directly with `file://` also works, but the AI then runs on the
> main thread instead of a Web Worker (browsers block workers on `file://`), which can
> make higher difficulties briefly stutter. Serve it over HTTP for the best experience.

## Structure

```
index.html        markup + UI shell
css/styles.css    theme, board, panels, animations
js/chess.js       rules engine (move generation, SAN, FEN, game state)
js/ai.js          evaluation + alpha-beta search
js/ai-worker.js   worker wrapper around the engine
js/app.js         board rendering, input, game flow, audio
```

## Notes

The engine is intentionally lightweight — a few hundred milliseconds per move at the
higher levels — so games feel snappy. Difficulty selects search depth, a time budget
and a small amount of move-selection noise, which keeps lower levels human and beatable.
