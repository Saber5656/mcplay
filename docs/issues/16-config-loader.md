# 16 — Config loader

## Title
Configuration discovery and loading (`src/core/config/config.ts`)

## Summary
Discover, load, validate, and merge configuration from `--config`, project, and global
locations over built-in defaults, per `docs/DESIGN.md` §13.

## Context
Commands need consistent defaults (scenarios dir, match mode, redaction, limits, log level)
resolvable from config and overridable by flags.

## Scope
- Discovery order: `--config` → `./mcplay.config.json` → XDG/global config.
- zod validation of the config shape (fields in DESIGN §13); invalid config fails closed
  (exit 3).
- Merge precedence: defaults < global < project < `--config` < CLI flags.
- Path fields validated via Issue 08.

## Detailed Requirements
- Provide `loadConfig({ configPath?, cliOverrides }): ResolvedConfig`.
- Defaults exactly as DESIGN §13 (`scenariosDir`, `defaultMatchMode`, redaction, limits,
  logLevel).
- Missing config files are not an error (use defaults); malformed ones are.

## Acceptance Criteria
- Unit test: precedence resolves correctly across layers.
- Unit test: malformed config → `ValidationError`.

## Validation
- Unit tests; consumed by most commands.

## Dependencies
- 01, 08, 11 (zod patterns).

## Non-goals
- No env-var-based config in v1 (documented).

## Design References
- `docs/DESIGN.md` §13; ADR-005.
