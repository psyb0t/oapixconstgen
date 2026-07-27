# Changelog

All notable changes per release. Versions follow [semver](https://semver.org).

## v1.1.5 — 2026-07-27

- Added a GitHub Actions CI status badge to the README.

## v1.1.4 — 2026-07-27

- Fix badges CI job — add needs dependency so the coverage badge waits for the
  coverage artifact.

## v1.1.3 — 2026-07-27

- Go 1.26; `make lint` now runs `go fix -diff` before `golangci-lint` (drops the
  `modernize` tool in favor of the standard library's own fixer).
- Added a coverage badge; `make test-coverage` now writes `coverage-percent.txt`
  and the pipeline's `badges` job publishes it.
- Errors returned across function boundaries wrap with `ctxerrors.Wrap` for
  file/line/function capture instead of `fmt.Errorf`.
- Logging already used `log/slog` directly (this is a CLI tool, not a service —
  no global logging configuration was added).

## v1.1.2 — 2026-07-27

- Added self-hosted version and license badges; wired a badges job into pipeline.yml.

## v1.1.1 — 2026-07-27

Add a LICENSE file.

- Add `LICENSE` (MIT). The project previously shipped without a license file; the
  tool's source is now explicitly MIT-licensed. Vendored dependencies under
  `vendor/` retain their own upstream licenses.

## v1.1.0 — 2026-03-29

- Add a `-file` flag for a custom output file name.

## v1.0.0 — 2026-01-28

Initial release. Extracts typed constants from an OpenAPI spec's `x-constants`
extension and generates a Go file with properly typed constants.
