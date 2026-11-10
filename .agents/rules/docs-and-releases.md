# Docs and releases

Prefix: DOC.

- **DOC1:** README links to repository files use full `https://github.com/psyb0t/oapixconstgen/blob/main/...` URLs, and links to the site use full `https://psyb0t.github.io/oapixconstgen/...` URLs.
- **DOC2:** A change to flags, exit codes, log messages, type keys, naming or formatting updates `docs/command-line.md` or `docs/x-constants.md` in the same change. Copy messages and defaults from `main.go`, never from another document.
- **DOC3:** Run `make docs` and the site checker after any docs change, and keep `docs/_includes/head_custom.html` and the two files under `docs/_sass/` as copied.
- **DOC5:** The annotated `vX.Y.Z` release tag's message carries that release's `CHANGELOG.md` section. The tag is the only version; there is no version file. Change `.github/workflows/pipeline.yml` together with the CI section of `docs/development.md`.
