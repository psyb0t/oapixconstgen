# Go style

Prefix: GO.

- **GO1:** `make lint` must pass: `go fix -diff ./...` prints nothing and golangci-lint with `.golangci.yml` reports 0 issues. Fix the code rather than adding a `//nolint`. The two existing ones (`depguard` on the package, `tagliatelle` on the `x-constants` tag) are there because the tool needs non-stdlib imports and a key name it does not choose.
- **GO2:** Errors returned from a function are wrapped with `ctxerrors.Wrap` and a short action phrase, as `generateCode` does. Do not use `fmt.Errorf` for wrapping.
- **GO3:** `run` reports a failure by logging one `ERROR` record (with an `error` field when there is an underlying error) and returning 1. Messages are lowercase and stable, because users search for them and `docs/command-line.md` lists each one. A new message or a changed one updates that table.
- **GO4:** Log through the `*slog.Logger` built in `run` on the writer it was given. Never write to stdout, and never call `os.Exit` outside `main`, so tests keep capturing everything through the buffer they pass in.
- **GO5:** Repeated literals are named constants (`filePerms`, `defaultOutFileName`, the `openAPIType*` keys). No magic numbers; `mnd` and `goconst` are enabled.
- **GO6:** Keep the tool to the standard library plus `gopkg.in/yaml.v3` and `github.com/psyb0t/ctxerrors`. A new dependency also lands in `vendor/` through `make dep`, and `vendor/` is never edited by hand.
