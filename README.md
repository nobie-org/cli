<p align="center">
  <a href="https://nobie.com/cli"><img src="https://nobie.com/favicons/nobie-logo.svg" width="88" alt="Nobie CLI documentation"></a>
</p>

<h1 align="center">Nobie CLI <sup>Beta</sup></h1>

<p align="center">
  <strong>Every agent now speaks xlsx.</strong><br>
  Full Excel compatibility for agents and machines.
</p>

<p align="center">
  <a href="https://nobie.com/cli">Documentation</a> ·
  <a href="https://nobie.com/install.sh">Install script</a> ·
  <a href="https://github.com/nobie-org/cli/issues">Issues</a>
</p>

nobie is a full Excel-compatible engine in your terminal. Pipe data in, compute formulas, style cells, query with SQL, and render to PNG or PDF. It composes with `jq`, `awk`, `curl`, `sqlite`, and the rest of your shell.

> [!NOTE]
> This repository is the GitHub home for Nobie CLI documentation and issues. It does not contain the CLI source.

## Install

On macOS or Linux:

```sh
curl https://nobie.com/install.sh | sh
```

For a noninteractive install, explicitly accept the [Nobie Desktop App License Terms](https://nobie.com/desktop-app-license-terms):

```sh
curl -fsSL https://nobie.com/install.sh | sh -s -- --accept-license
```

The installer supports x86-64 and ARM64. On macOS it installs `Nobie.app` and the `nobie` command; on Linux it installs the `nobie` binary.

Verify the installation:

```sh
nobie version
```

## The whole spreadsheet, headless and pipeable

- **Compute — Every formula calculated correctly.** Set a cell to `=PMT(…)` or `=XLOOKUP(…)` and read the answer back. It's the same engine the app runs, so if Excel can compute it, so can a shell script.
- **Pipe — Text in, text out.** Commands read stdin and write stdout where data flows through the shell, so a workbook works with `jq`, `awk`, `curl`, `sqlite`, and everything else you already use.
- **Render — Pixel perfect rendering.** Render any range to a PNG, or a whole workbook to a PDF, drawn the way the app would draw it.

## Your first sheet, start to finish

Create a workbook:

```sh
nobie workbook new ./budget.xlsx
```

Write a label, a value, and a formula. Writes are explicit: `--overwrite` updates the target workbook; `--save-to PATH` writes a distinct result; without either option the change is ephemeral.

```sh
nobie cell set ./budget.xlsx Sheet1!A1 --value Revenue --overwrite
nobie cell set ./budget.xlsx Sheet1!B1 --value 1000 --overwrite
nobie cell set ./budget.xlsx Sheet1!B2 --value '=B1*1.2' --overwrite
```

Read the evaluated result, then the authored formula:

```console
$ nobie cell read ./budget.xlsx Sheet1!B2
1200
$ nobie cell read ./budget.xlsx Sheet1!B2 --inputs
=B1*1.2
```

Read the same cells as structured JSON:

```console
$ nobie cell read ./budget.xlsx Sheet1!A1:B2 --json
{"schema":"nobie.cells.v1","projection":"values","cells":[{"ref":"Sheet1!A1","value":"Revenue"},{"ref":"Sheet1!B1","value":"1000"},{"ref":"Sheet1!B2","value":"1200"}]}
```

Render the range to PNG or the whole workbook to PDF:

```sh
nobie range render ./budget.xlsx Sheet1!A1:B2 --out preview.png
nobie workbook print ./budget.xlsx --out budget.pdf
```

<sub>Examples verified with stable Nobie 0.1.3 on July 15, 2026.</sub>

## One grammar for workbook commands

Most workbook operations follow the same shape:

```text
nobie <noun> <verb> <TARGET> [SELECTOR] [FLAGS]
```

- A **target** identifies a workbook: a path such as `./report.xlsx`, or an `@Report.xlsx` handle held open for repeated operations.
- A **selector** identifies cells: `Sheet1!A1`, `Sheet1!A1:C9`, or a bare `A1` paired with `--sheet Sheet1`.
- `--json` produces structured output where supported; `--inputs` reads authored formulas instead of evaluated values.

The CLI's own help is the current command reference:

```sh
nobie --help
nobie cell --help
nobie cell set --help
```

See the complete [Nobie CLI documentation](https://nobie.com/cli) for sessions, spreadsheet scripts, styles, calculation modes, HTTP APIs, and the full command reference.

## Issues and license

Use [GitHub Issues](https://github.com/nobie-org/cli/issues) for Nobie CLI questions and bug reports.

The Nobie CLI is licensed, not sold. Its use is governed by the [Nobie Desktop App License Terms](https://nobie.com/desktop-app-license-terms).

Copyright © 2026 Nobie, Inc. All rights reserved.
