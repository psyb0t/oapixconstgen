# Docs and releases

Prefix: DOC.

- **DOC1:** `README.md` is the front door: what the tool is, install, one example, the flag list in one sentence, and the documentation link table. Reference material lives in `docs/` and each fact has one home. README links to repository files use full `https://github.com/psyb0t/oapixconstgen/blob/main/...` URLs, and links to the site use full `https://psyb0t.github.io/oapixconstgen/...` URLs.
- **DOC2:** A change to flags, exit codes, log messages, type keys, naming or formatting updates `docs/command-line.md` or `docs/x-constants.md` in the same change. Copy messages and defaults from `main.go`, never from another document.
- **DOC3:** Every page in `docs/` has front matter with a unique `title` and a `description` of 50 to 170 characters, and one `#` heading. Run `make docs` and the site checker after any docs change, and keep `docs/_includes/head_custom.html` and the two files under `docs/_sass/` as copied.
- **DOC4:** Public prose has no em dashes, says "directory" rather than "folder", and speaks as one person ("I"), never "we".
- **DOC5:** A release is a commit on `main` with a new `CHANGELOG.md` section, then an annotated `vX.Y.Z` tag on it whose message carries that section. The tag is the only version; there is no version file. Change `.github/workflows/pipeline.yml` together with the CI section of `docs/development.md`.
