# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

`linear-browse` is a single self-contained POSIX shell script (`./linear-browse`) that provides an interactive terminal UI for browsing Linear issues. It uses fzf for the interactive interface, linear-cli for API access, jq for JSON parsing, perl for text formatting, and optionally glow for rendered markdown previews.

## Architecture

The script uses a subcommand dispatch pattern — it re-invokes itself with internal flags (`--preview`, `--detail`, `--list`, `--pick-project`, `--pick-team`, `--fmt`) from within fzf bindings. The `$SELF` variable holds the script's own path for this purpose. Session state (selected team/project) is persisted via temp files in `$TMPDIR`.

Key flow: main entrypoint → dependency checks → resolve team → resolve project → launch fzf with bindings that call back into the script.

## Running

```sh
# Launch (requires linear-cli auth + fzf ≥0.54 + jq + perl)
./linear-browse

# Check version
./linear-browse --version
```

No build step, no tests, no linting. The script is the entire project.

## Development Notes

- Must remain POSIX sh compatible (`#!/bin/sh`, `set -eu`). No bashisms.
- The `eval` in `do_list` is intentional — it handles dynamic flag construction with quoted project names.
- fzf `become()` is used for full re-launch (team/project switching, reset), which requires fzf ≥0.54.
- Text formatting in `do_fmt` uses perl regex to reformat linear-cli's markdown output for better terminal display.
