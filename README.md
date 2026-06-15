# MU Server Custom Config Syntax

VSCode extension providing syntax highlighting for MU server custom configuration files (`.dat.txt`).

## Features

- **Blocks** — lines with a single number open a block, `end` closes it
- **Comments** — `//` line comments
- **Numbers** — all numeric values highlighted
- **Strings** — double-quoted `"..."` and single-quoted `'...'` strings

## File Format

```
1          // block header
3 12 5     // data row — numbers highlighted
// comment line
2 "title"  // mixed numbers and strings
end        // block terminator
```

## Installation

Open the `mu-custom-config-syntax` folder in VSCode and press **F5** (Extension Development Host), or copy this folder to `.vscode/extensions/`.

## Usage

Any file ending with `.dat.txt` is automatically recognized. Alternatively, open the command palette and select **MU Custom Config** from the language picker.
