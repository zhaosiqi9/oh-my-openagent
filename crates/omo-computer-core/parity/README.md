# Parity ledgers of the computer-use v3 reimplementation

Base: `f922d6ac3a78be2206e00d7ae433593d5faf51c7` (the old tree's reference commit, `$CU_RUN/BASE` of the executor).

## test-parity-manifest.tsv

One row per old test, measured at the base: `unit`, `kind` (rust | vitest | bun), `test id`, `status`, `note`.

| kind | rows | measured as |
|---|---|---|
| rust | undefined | every `#[test]` / `#[tokio::test]` attribute under `crates/senpi-desktop-*`; id = `<crate>::<module path>::<fn>`, the module path read from the file path and the enclosing `mod` blocks; one attribute sits in a `macro_rules!` template and is recorded with its `$name` token |
| vitest | undefined | every `it(` / `test(` title (modifiers such as `.each(...)` included) in `packages/senpi-desktop-*/test/**/*.test.ts` (51 files); id = `<package>/<file>::<title>` |
| bun | undefined | every `it(` / `test(` title (modifiers such as `.if(...)` included) in the 35 `*.test.ts` files under a class B path of the Removal inventory; id = `<file>::<title>` |

Measurement note: `git grep -c -E '^\s*#\[(tokio::)?test'` reports undefined on a Linux host (glibc ERE
knows `\s`) but 1182 on macOS, where git's ERE treats `\s` literally and the pattern only matches attributes at
column 0; `git grep -c -P '^\s*#\[(tokio::)?test'` or `-E '^[[:space:]]*#\[(tokio::)?test'` reports
undefined on both. The plan's figure of 1,182 is that macOS artefact; the manifest carries every attribute.

A title that repeats inside one file carries a ` #<n>` suffix so every id is unique. A later row sets `status` to
`ported:<new test id>`, `superseded:AC-<n>` or `dropped:<reject-table row>` in the pull request that replaces the test;
`pending` is refused by `bun script/computer-v3-guards.ts --parity --final` (row 74).

## DEFERRED-LEGS.tsv

`design row`, `leg`, `recorded by row`, `owed to row`, `status` (`owed` | `passed:<test or scenario id>`); Executor
contract item 14 of the plan says how rows use it. No leg may still read `owed` to row 74 or earlier at the parity gate.

## ORACLE-PROVENANCE.tsv

Written by row 3 (#13025): one row per imported fixture (new path, old path, base sha, old sha256, new sha256, substitutions).
