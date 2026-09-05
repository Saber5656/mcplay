# 01 — Project scaffolding & build toolchain

## Title
Project scaffolding & build toolchain (TypeScript, ESM, npm bin `mcplay`)

## Summary
Create the TypeScript/Node project skeleton, build pipeline, linting/formatting, and the
`mcplay` CLI bin stub, matching the module layout in `docs/DESIGN.md` §5.

## Context
mcplay is a CLI-first tool implemented in TypeScript on Node.js 20+, distributed via npm
(ADR-001, ADR-002). Every later issue builds on this skeleton. The layout must be created up
front so subsequent issues drop files into stable paths.

## Scope
- `package.json` (name `mcplay`, `type: module`, `bin: { mcplay: dist/cli/index.js }`,
  `engines.node >= 20`, scripts: `build`, `typecheck`, `lint`, `format`, `test`).
- `tsconfig.json` (strict, ESM/NodeNext, `outDir dist`, `rootDir src`).
- ESLint + Prettier config.
- Empty directory skeleton per DESIGN §5 with `index.ts` placeholders that compile.
- `src/cli/index.ts` bin stub printing `--version`/`--help` (real dispatch in later issues).
- `.gitignore`, `LICENSE` (MIT unless human specifies otherwise — note as assumption),
  `.nvmrc`/`.node-version` = 20.

## Detailed Requirements
- Use `commander` for arg parsing (declare dependency; do not implement subcommands yet).
- Build to ESM in `dist/`; the bin file has a shebang `#!/usr/bin/env node` after build.
- `npm run build && node dist/cli/index.js --version` prints the package version.
- Dependencies pinned; commit the lockfile.
- No product logic beyond the version/help stub.

## Acceptance Criteria
- `npm ci && npm run build && npm run typecheck && npm run lint` all succeed.
- `node dist/cli/index.js --help` exits 0 and lists (placeholder) usage.
- Directory skeleton matches DESIGN §5 (dirs exist, compile clean).

## Validation
- CI (Issue 05) runs build/typecheck/lint.
- Manual: `npx .` after build prints version.

## Dependencies
- None.

## Non-goals
- No subcommand behavior, no MCP SDK usage yet.

## Design References
- `docs/DESIGN.md` §5 (layout), §6 (CLI conventions); ADR-001; ADR-002.
