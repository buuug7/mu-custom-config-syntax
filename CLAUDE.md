# CLAUDE.md — mu-custom-config-syntax

This file provides guidance to Claude Code when working on this extension.

## Project Overview

This is a **VSCode TextMate grammar extension** that provides syntax highlighting for MU Online server custom configuration files (`.dat.txt`). It's a companion to the main [mu-server](e:\Main5.2\mu-server) project.

## What It Highlights

The extension targets files with the `.dat.txt` extension. These files have a simple custom format:

- **Block sections**: A line containing only a number starts a block; `end` (on its own line) closes it
- **Numeric values**: All numbers highlighted as `constant.numeric`
- **String values**: Double-quoted `"..."` and single-quoted `'...'` strings
- **Line comments**: `//` to end of line

## File Structure

| File | Purpose |
|------|---------|
| `package.json` | Extension manifest. Registers:
  - Language `mu-custom-config` (aliases: `MU Custom Config`, `MU Custom`)
  - File match pattern: `**/*.dat.txt`
  - Grammar at `syntaxes/mu-custom-config.tmLanguage.json`
  - Language config at `language-configuration.json` |
| `language-configuration.json` | Basic VSCode language config: `//` as line comment, bracket pairs `()`, `[]`, `{}` |
| `syntaxes/mu-custom-config.tmLanguage.json` | **TextMate grammar** — the core of the extension |
| `.vscode/launch.json` | F5 launch config for extension development host |
| `README.md` | User-facing docs |

## Grammar Details (`tmLanguage.json`)

**Scope name**: `source.mu-custom-config`

Example files for visual testing:
- [`example.dat.txt`](example.dat.txt) — comprehensive reference
- [`example1.dat.txt`](example1.dat.txt) — standard blocks
- [`example2.dat.txt`](example2.dat.txt) — headerless block (just data rows + `end`)
- [`example3.dat.txt`](example3.dat.txt) — blocks with strings and comments

The grammar has six top-level patterns in order:

1. **Block section** (meta scope `meta.block.mu-custom-config`):
   - **Begin**: `^\s*\d+\s*$` — block header → `keyword.control.mu-custom-config`
   - **End**: `^\s*end\s*$` — block terminator → `keyword.control.mu-custom-config`
   - **Content patterns** (only matched inside blocks):
     - `//` comments → `comment.line.double-slash.mu-custom-config`
     - `"..."` strings → `string.quoted.double.mu-custom-config`
     - `'...'` strings → `string.quoted.single.mu-custom-config`
     - Numbers → `constant.numeric.mu-custom-config`

2. **Top-level comments** — `//` outside blocks → `comment.line.double-slash`

3. **Top-level double-quoted strings** — `"..."` → `string.quoted.double`

4. **Top-level single-quoted strings** — `'...'` → `string.quoted.single`

5. **Top-level numbers** — `\b\d+\b` → `constant.numeric`

6. **Top-level `end` fallback** — `^\s*end\s*$` → `keyword.control`. Catches stray `end` outside blocks (e.g. headerless files like `example2.dat.txt`).

Note: Content patterns (comments, strings, numbers) are duplicated inside and outside the block. This is by design — TextMate begin/end patterns only match their end pattern within the same block, so top-level versions are needed for content outside blocks.

## Development

- **Edit** the `.tmLanguage.json` file to adjust the grammar
- **F5** in VSCode → opens Extension Development Host to test
- The extension auto-activates when you open any file matching `**/*.dat.txt`
- Manual language picker: select `MU Custom Config`

## Relationship to mu-server

| Context | Details |
|---------|---------|
| Config files rendered by this extension | `e:\Main5.2\mu-server\Data\Custom\*.txt` (named `*.dat.txt` in the extension convention) |
| Config encoding | All `.txt` files are **GBK (codepage 936)** — but the grammar extension is encoding-agnostic (VSCode handles display) |
| Source that reads these configs | `e:\Main5.2\Source_MuServer\GameServer\GameServer\Custom*.cpp` |
| Config format origin | Chinese MuOnline private server custom features |

## Memory

Personal/memory files are stored at `C:\Users\buuug7\.claude\projects\e--Main5-2-mu-server-mu-custom-config-syntax\memory\`.
