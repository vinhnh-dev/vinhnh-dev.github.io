---
layout: page
title: Vim CheatSheet
permalink: /vim/
parent: Miscellaneous
nav_order: 1
---

# Vim Cheat Sheet

## Moving while in Insert mode

If you don’t want to leave Insert mode, many terminals support word-wise movement:

- **Ctrl + Right Arrow**: move forward one word
- **Ctrl + Left Arrow**: move backward one word
- **Ctrl + o**, then **w**: execute one Normal-mode command (`w`) and return to Insert mode

## Word motions (Normal mode)

Make sure you are in Normal mode (press `Esc` if needed).

### Core word motions

- `w`: move forward to the beginning of the next word
- `e`: move forward to the end of the current or next word
- `b`: move backward to the beginning of the current or previous word
- `ge`: move backward to the end of the previous word

### `word` vs `WORD`

Vim distinguishes between:

- `word`: letters, numbers, underscores
- `WORD`: any non-blank characters separated by whitespace

Related motions:

- `W`: jump to the next `WORD` (skips punctuation like `.` or `/`)
- `B`: jump back one `WORD`
- `E`: jump to the end of the current `WORD`

### Moving multiple words

Prefix motions with a number:

- `3w`: move forward 3 words
- `5b`: move backward 5 words

## Visual selections

### Select character-by-character (Visual mode)

- Press `v` to enter Visual mode
- Move the cursor to expand the selection

### Select multiple lines (Visual Line mode)

- Press `V` (Shift + V)
- Use `j` / `k` to expand the selection
- Actions:
  - `y`: yank (copy)
  - `d`: delete (cut)
  - `>`: indent right

### Select a column/block (Visual Block mode)

- Press `Ctrl + V`
- Use `h`, `j`, `k`, `l` to select a rectangle
- Multi-line insert:
  - Press `I` (Shift + I)
  - Type text
  - Press `Esc` to apply to all selected lines

### Quick command summary

| Task | Command |
| --- | --- |
| Start selecting lines | `V` |
| Start selecting block | `Ctrl + V` |
| Copy (yank) selection | `y` |
| Cut (delete) selection | `d` |
| Select 10 lines down | `10j` (in Visual mode) |

Note: Standard Vim does not support selecting non-adjacent lines. For that, use commands like `:g/pattern/command` or a plugin such as `vim-visual-multi`.

## Jumping by `WORD`s

- `W`: jump to the start of the next `WORD`
- `B`: jump backward to the start of the previous `WORD`

## Moving to the next/previous blank line

- `}`: jump to the next empty line (end of the current paragraph)
- `{`: jump to the previous empty line

## Moving to the next whitespace

### On the same line

- `f<Space>`: find the next space on the current line
- `t<Space>`: move the cursor right before the next space
- `;`: repeat the last `f` / `t` search forward
- `,`: repeat the last `f` / `t` search backward
- `E`: jump to the end of the current space-delimited `WORD`

### Across lines (search)

- `/\s`: search for whitespace (spaces, tabs, newlines)
- `/ `: search for a literal space
- `n`: next match
- `N`: previous match

## NERDTree focus and navigation

### Opening files (and controlling focus)

- `o`: open file, move focus to the file
- `go`: open file, keep focus in NERDTree
- `t`: open in a new tab, move focus to it
- `T`: open in a new tab, keep focus in NERDTree
- `i` / `s`: open in horizontal/vertical split, move focus
- `gi` / `gs`: open in horizontal/vertical split, keep focus in NERDTree

### Move focus to the NERDTree window

- `:NERDTreeFocus`: jump to the NERDTree window
- `Ctrl + w`, then `h`: move to the left window (if NERDTree is on the left)
- `Ctrl + w`, then `w`: cycle through windows
- `Ctrl + w`, then `p`: go to the previously accessed window

### Navigating the tree

- `Ctrl + j`: next sibling
- `Ctrl + k`: previous sibling
- `J`: last child
- `K`: first child
- `P`: go to parent
- `C`: root to current node
- `u`: move root up one directory