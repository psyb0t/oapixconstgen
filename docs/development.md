---
title: "Development"
description: "Build, lint and test oapixconstgen with its Make targets, the 90 percent coverage gate, the vendored golangci-lint, and how CI and releases run."
nav_order: 4
---

# Development

Working on oapixconstgen itself. You need Go 1.26 or newer and Make. golangci-lint comes from the module's own `tool` directive in [`go.mod`](https://github.com/psyb0t/oapixconstgen/blob/main/go.mod), and every dependency is vendored in [`vendor/`](https://github.com/psyb0t/oapixconstgen/tree/main/vendor), so there is nothing to install globally. Building the documentation site needs Docker.

## Make targets

Run `make help` for the list. The [`Makefile`](https://github.com/psyb0t/oapixconstgen/blob/main/Makefile) has:

| Target | What it does |
| --- | --- |
| `make all` | Runs `dep`, `lint-fix` and `test-coverage`, in that order. |
| `make dep` | `go mod tidy` and `go mod vendor`. |
| `make lint` | Fails if `go fix -diff ./...` has suggestions, then runs `go tool golangci-lint run ./...` with the rules in [`.golangci.yml`](https://github.com/psyb0t/oapixconstgen/blob/main/.golangci.yml). |
| `make lint-fix` | Applies `go fix ./...` and runs golangci-lint with `--fix`. |
| `make test` | `go test -race ./...`. |
| `make test-coverage` | Runs the tests with the race detector and a coverage profile, writes the total to `coverage-percent.txt`, and fails below 90 percent. |
| `make docs` | Builds this site with GitHub's own Pages image into `.testing/docs-site`. |

If your environment sets `GOFLAGS=-mod=readonly`, `go fix -diff` may print a `go: downloading` line the first time it runs, and `make lint` treats that as a finding. Run it with `GOFLAGS=` so Go uses the vendored modules.

## Layout

| Path | Holds |
| --- | --- |
| [`main.go`](https://github.com/psyb0t/oapixconstgen/blob/main/main.go) | The whole tool: flag parsing, spec loading, `collectConstants`, `generateCode` and `toExportedName`. |
| [`main_test.go`](https://github.com/psyb0t/oapixconstgen/blob/main/main_test.go) | Table tests for `run` and each helper. |
| [`.fixtures/api.yml`](https://github.com/psyb0t/oapixconstgen/blob/main/.fixtures/api.yml) | A spec that uses every supported type key, read by the tests. |
| [`docs/`](https://github.com/psyb0t/oapixconstgen/tree/main/docs) | This site, built by GitHub Pages from the `main` branch. |

The tests are in `package main` and call `run` directly with an argument list and a buffer for the log output, so no binary is built.

## CI

[`.github/workflows/pipeline.yml`](https://github.com/psyb0t/oapixconstgen/blob/main/.github/workflows/pipeline.yml) runs on every push.

- On a branch push, the shared Go workflow runs `make lint`, `make test-coverage` and govulncheck.
- When that run is on `main` and passes, the badges job publishes the coverage, version and license badges to the `badges` branch.
- On a `v*` tag, the pipeline does not rerun the checks. It waits for the `main` run of the same commit to pass, cuts the GitHub release with the annotated tag message as its notes, and refreshes the badges with the coverage from that `main` run. The `main` run keeps its coverage artifact for one day, so a tag pushed later than that still gets its release, but the badges job fails.

The other workflows are housekeeping. `mirror-and-archive.yml` mirrors every push to GitLab and Codeberg and archives the default branch and tags. `issue-pull.yml` copies issues filed on those mirrors into GitHub every six hours. `collaborators-only.yml` closes and locks pull requests from anyone who is not a collaborator and points them at the issue tracker.

## Releases

Versions follow [semver](https://semver.org). The Git tag is the version; there is no version file. A release is a commit on `main` that has passed CI with its [CHANGELOG](https://github.com/psyb0t/oapixconstgen/blob/main/CHANGELOG.md) entry, then an annotated `vX.Y.Z` tag on that commit. `go install github.com/psyb0t/oapixconstgen@latest` picks up the new tag through the Go module proxy.
