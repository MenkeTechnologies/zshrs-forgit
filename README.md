```
███████╗ ██████╗ ██████╗  ██████╗ ██╗████████╗
██╔════╝██╔═══██╗██╔══██╗██╔════╝ ██║╚══██╔══╝
█████╗  ██║   ██║██████╔╝██║  ███╗██║   ██║   
██╔══╝  ██║   ██║██╔══██╗██║   ██║██║   ██║   
██║     ╚██████╔╝██║  ██║╚██████╔╝██║   ██║   
╚═╝      ╚═════╝ ╚═╝  ╚═╝ ╚═════╝ ╚═╝   ╚═╝   
                                              
```

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![zshrs plugin](https://img.shields.io/badge/zshrs-native%20plugin-blue.svg)](https://github.com/MenkeTechnologies/zshrs)

### `[INTERACTIVE GIT + FZF — COMPILED]`

> *"git add, log, diff, stash — native builtins, not sourced functions."*

## `[NATIVE ZSHRS PLUGIN]`

[forgit](https://github.com/wfxr/forgit) — the interactive `git` + `fzf` utility — ported to a **native [zshrs](https://github.com/MenkeTechnologies/zshrs) plugin**. Instead of shell functions parsed on every startup, the commands are compiled Rust builtins in a `cdylib` loaded through zshrs's stable plugin ABI with `zmodload -R`.

### [`zshrs`](https://github.com/MenkeTechnologies/zshrs) &middot; [`znative`](https://github.com/MenkeTechnologies/zshrs/blob/main/docs/ZNATIVE.md) &middot; [`upstream`](https://github.com/wfxr/forgit)

---

## Table of Contents

- [\[0x00\] Overview](#0x00-overview)
- [\[0x01\] Install](#0x01-install)
- [\[0x02\] Commands](#0x02-commands)
- [\[0x03\] How it was ported](#0x03-how-it-was-ported)
- [\[0xFF\] License](#0xff-license)

---

## [0x00] OVERVIEW

`ga`, `glo`, `gd`, … the whole forgit command set, running as native machine code with no per-startup sourcing. Orchestration, argument handling, and sequencing are Rust; `git` and `fzf` run as subprocesses exactly as upstream.

Requires `git` and `fzf` on `PATH` (same runtime deps as upstream forgit). Diffs are rendered through whatever pager you configure — `$FORGIT_PAGER`, else `git config core.pager`, else `cat` — so pointing that at `delta` or `diff-so-fancy` works, but neither is auto-detected.

---

## [0x01] INSTALL

```sh
znative load MenkeTechnologies/zshrs-forgit
```

Put that one line in your `.zshrc`. [znative](https://github.com/MenkeTechnologies/zshrs/blob/main/docs/ZNATIVE.md), zshrs's package manager, installs the plugin on the first shell start — clones it, runs `cargo build --release`, and `zmodload -R`s the resulting `libforgit` — then loads it from the store, zero-network, on every start after. No separate install step.

### Manual build

```sh
cargo build --release
zmodload -R ./target/release/libforgit.dylib   # .so on Linux
ga
```

---

## [0x02] COMMANDS

| alias    | what it does                        |
| -------- | ----------------------------------- |
| `ga`     | interactive `git add`               |
| `grh`    | unstage files (`git reset HEAD`)    |
| `glo`    | commit log viewer                   |
| `gd`     | diff viewer                         |
| `gcf`    | checkout / restore modified files   |
| `gclean` | `git clean` selector                |
| `gss`    | stash viewer                        |
| `gcp`    | cherry-pick selector                |
| `gi`     | `.gitignore` template generator     |

### Environment

| variable                   | effect                                                                |
| -------------------------- | --------------------------------------------------------------------- |
| `FORGIT_PAGER`             | pager for all diff output; falls back to `git config core.pager`, then `cat` |
| `FORGIT_SHOW_PAGER`        | pager for `git show`; falls back to `git config pager.show`, then `$FORGIT_PAGER` |
| `FORGIT_DIFF_PAGER`        | pager for `git diff`; falls back to `git config pager.diff`, then `$FORGIT_PAGER` |
| `FORGIT_FZF_DEFAULT_OPTS`  | appended to the built-in fzf option block (after `$FZF_DEFAULT_OPTS`)  |
| `FORGIT_COPY_CMD`          | command used to copy a selection to the clipboard (default `pbcopy`) |
| `FORGIT_GI_REPO_REMOTE`    | `.gitignore` template repo URL used by `gi` (default `https://github.com/dvcs/gitignore`) |
| `FORGIT_GI_REPO_LOCAL`     | local clone path for that template repo (default `~/.forgit/gi/repos/dvcs/gitignore`) |
| `FORGIT_GI_TEMPLATES`      | template directory (default `$FORGIT_GI_REPO_LOCAL/templates`) |

---

## [0x03] HOW IT WAS PORTED

A worked example for the zshrs plugin porting guide: [docs/PORTING_ZSH_PLUGIN.md](https://github.com/MenkeTechnologies/zshrs/blob/main/docs/PORTING_ZSH_PLUGIN.md). Orchestration, argument handling, and sequencing are Rust; `git`/`fzf` run as subprocesses; fzf `--preview` strings stay shell (fzf runs them via `sh`).

---

## [0xFF] LICENSE

MIT. Ported from [wfxr/forgit](https://github.com/wfxr/forgit) (MIT). See [LICENSE](LICENSE).
