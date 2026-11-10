# Go style

Prefix: GO.

- **GO1:** The two existing `//nolint` directives (`depguard` on the package, `tagliatelle` on the `x-constants` tag) are there because the tool needs non-stdlib imports and a key name it does not choose.
- **GO3:** `run` reports a failure by logging one `ERROR` record (with an `error` field when there is an underlying error) and returning 1. Messages are lowercase and stable, because users search for them and `docs/command-line.md` lists each one. A new message or a changed one updates that table.
- **GO4:** Log through the `*slog.Logger` built in `run` on the writer it was given. Never write to stdout, and never call `os.Exit` outside `main`, so tests keep capturing everything through the buffer they pass in.
- **GO6:** Keep the tool to the standard library plus `gopkg.in/yaml.v3` and `github.com/psyb0t/ctxerrors`.
