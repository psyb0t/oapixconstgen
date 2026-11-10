# Testing

Prefix: TEST. How to run the suites is in `AGENTS.md` and `docs/development.md`.

- **TEST1:** Tests are table-driven, in `package main`, in `main_test.go`, using only the standard `testing` package. Keep that style; do not add testify for this file.
- **TEST2:** CLI behavior is tested through `run(args, &buf)` with a temporary directory from the case's `setup`, never by building and executing the binary. Every exit path of `run` has a case (missing flag, bad flag, unreadable spec, bad YAML, format failure, write failure, no constants, success).
- **TEST3:** Cases that read `.fixtures/api.yml` use a path relative to the repository root, since `go test` runs in the package directory. A new fixture goes in `.fixtures/`.
- **TEST4:** `make test-coverage` fails below `MIN_TEST_COVERAGE` (90).
- **TEST5:** Every example in the README and in `docs/` that shows a spec and its output must match what the tool prints for that spec. Run the tool on the example after changing either side.
