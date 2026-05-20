# poetry-single-int-version-whl

Regression test for **SCA-5540** — `versionPatternWhl` regex fix.

## What this tests

The **direct dependency path** for a calendar-versioned package: `pdfminer.six==20251230` is declared
directly in `pyproject.toml`. This complements `poetry_calendar_version_whl` which covers the
*transitive* path via `pdfplumber`.

## Bug background

`AbstractPythonDependencyResolver.versionPatternWhl` required at least one dot in the wheel filename
version segment. A calendar version like `20251230` has no dots, causing a stripped dep record
(no `sha1`, `systemPath`, `filename`, `dependencyFile`).

## Key assertion

`pdfminer-six` must appear in `direct_dependencies_names_list` with a non-empty `sha1`.
