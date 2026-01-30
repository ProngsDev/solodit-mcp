# Repository Guidelines

## Project Structure & Module Organization

- `src/` contains the TypeScript source; `src/index.ts` is the MCP server entry point.
- `dist/` holds compiled JavaScript output from `tsc` and is generated.
- Root config files include `package.json`, `tsconfig.json`, `eslint.config.js`, and Prettier configs.
- Documentation lives in `README.md`, `USAGE.md`, and `QUICK_START.md`.

## Build, Test, and Development Commands

- `npm run dev` runs the server directly from TypeScript via `tsx`.
- `npm run build` compiles TypeScript into `dist/`.
- `npm run start` runs the compiled server (`node dist/index.js`).
- `npm run watch` runs the TypeScript compiler in watch mode.
- `npm run lint` and `npm run lint:fix` run ESLint checks and fixes.
- `npm run format` and `npm run format:check` run Prettier formatting.
- `npm run type-check` runs `tsc --noEmit` for type validation.

## Coding Style & Naming Conventions

- Formatting is handled by Prettier (2-space indentation, semicolons, double quotes, 80-char line width).
- Linting is enforced via ESLint with TypeScript support.
- Use camelCase for variables/functions, PascalCase for types/classes, and kebab-case for filenames where applicable.
- Keep changes focused in `src/` and avoid editing generated `dist/` files.

## Testing Guidelines

- There are no unit tests in this repository today; rely on `npm run type-check`, `npm run lint`, and `npm run build` as the primary quality gates.
- If you add tests in the future, place them under a `tests/` or `__tests__/` directory and document the command in `package.json`.

## Commit & Pull Request Guidelines

- Commit messages follow Conventional Commits based on history, e.g., `chore: ...`, `docs: ...`, `refactor: ...`.
- Keep PRs focused, include a short summary, and list commands run (e.g., `npm run lint`, `npm run build`).
- Link related issues when applicable and describe any configuration changes (e.g., new env vars).

## Security & Configuration Tips

- The server requires `SOLODIT_API_KEY` in the environment; avoid committing keys.
- The Solodit API rate limit is 20 requests per 60 seconds; consider pagination and backoff.
- Node.js 18+ is required for local development.
