# Soc Ops - AI Coding Agent Instructions

Social Bingo game for in-person mixers. React 19 + TypeScript + Vite + Tailwind CSS v4.

## Development Checklist (MANDATORY before commits)
- [ ] `npm run lint` - No ESLint errors
- [ ] `npm run build` - TypeScript compiles successfully
- [ ] `npm test` - All Vitest tests pass

## Architecture

**State**: Single custom hook (`useBingoGame`) with localStorage persistence
- `src/hooks/useBingoGame.ts` - Game lifecycle (start → playing → bingo), uses `queueMicrotask()` for async state updates
- Validates stored data with version checks, SSR-safe guards (`typeof window`)

**Components**: `App` → `StartScreen` | `GameScreen` (→ `BingoBoard` → `BingoSquare`, `BingoModal`)

**Logic**: Pure functions in `src/utils/bingoLogic.ts` (immutable transformations)
- `generateBoard()` - Fisher-Yates shuffle, center (index 12) is FREE SPACE
- `toggleSquare()` - Returns new array for React immutability
- `checkBingo()` - Tests 12 winning lines (5 rows, 5 cols, 2 diagonals)

## Key Conventions

**TypeScript**: Domain types in `src/types/index.ts` (BingoSquareData, GameState, BingoLine), re-exported from utils. Props interfaces co-located with components.

**React**: Functional components only, hooks for state. Custom hook returns `{state, actions}` pattern.

**Tailwind v4**: `@theme` in `src/index.css` (no config file). Use `bg-accent`, `border-marked-border`, `bg-white/10` (not `bg-opacity-10`).

**Game**: 5x5 board (hardcoded), 24 questions in `src/data/questions.ts` (shuffled per game), 5-in-a-row wins.

## Commands

```bash
npm run dev   # http://localhost:5173
npm test      # Vitest (watch disabled, CI mode)
npm run build # Auto-deploys to GitHub Pages on push to main
```

## Key Files

- `src/hooks/useBingoGame.ts` - Game state machine
- `src/utils/bingoLogic.ts` - Board operations
- `src/data/questions.ts` - Customize questions here
- `.github/instructions/frontend-design.instructions.md` - Avoid AI slop
- `.github/instructions/tailwind-4.instructions.md` - v4 syntax
