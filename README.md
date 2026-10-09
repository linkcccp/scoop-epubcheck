# scoop-epubcheck

A [Scoop](https://scoop.sh) bucket providing a self-contained [EPUBCheck](https://github.com/w3c/epubcheck) package for Windows.

Every release bundles everything needed to run EPUBCheck — no system Java, no configuration:

- `epubcheck.cmd` — launcher (shim-friendly)
- `epubcheck.jar` + `lib/` — EPUBCheck itself
- `jre/` — private Eclipse Temurin JRE (Windows x64)
- `licenses/`, `LICENSE.txt`, `THIRD-PARTY.txt` — upstream license files

## Install

```powershell
scoop bucket add scoop-epubcheck https://github.com/linkcccp/scoop-epubcheck
scoop install epubcheck
```

## Usage

```powershell
epubcheck my-book.epub
epubcheck --version
```

## How updates work

A scheduled GitHub Action ([update-epubcheck.yml](.github/workflows/update-epubcheck.yml)) runs daily:

1. Detects the latest upstream EPUBCheck release and the latest Temurin 21 JRE.
2. Downloads both, verifies checksums, and assembles a bundled release archive.
3. Smoke-tests the bundle (runs `epubcheck --version`, validates a broken EPUB fails).
4. Publishes a release tagged `v<epubcheck>-jre<jre>` (e.g. `v5.4.0-jre21.0.12.1`) and updates [bucket/epubcheck.json](bucket/epubcheck.json).

The manifest can also be refreshed manually with `scoop update` / `scoop status` after a release is published.

## Development

- `bin\checkver.ps1 epubcheck` — compare the manifest version with published releases (add `-Update` to rewrite it)
- `bin\test.ps1` — run the official Scoop bucket test suite: manifest schema validation plus repository style checks (requires PowerShell Pester ≥ 5.2 and BuildHelpers)

## Disclaimer

Unofficial packaging. EPUBCheck is developed by the [W3C](https://www.w3.org/); this bucket is not affiliated with or endorsed by the W3C or the Eclipse Adoptium project.
