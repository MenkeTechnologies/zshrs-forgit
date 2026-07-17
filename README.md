# zshrs-forgit

[forgit](https://github.com/wfxr/forgit) — the interactive `git` + `fzf`
utility — ported to a **native [zshrs](https://github.com/MenkeTechnologies/zshrs)
plugin**. Instead of shell functions parsed on every startup, the commands
are compiled Rust builtins in a `cdylib` loaded with `zmodload -R`.

## Commands

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

Requires `git` and `fzf` on `PATH` (same runtime deps as upstream forgit).
`delta` / `diff-so-fancy` are used for diff rendering when present.

## Install

With **zpm** (zshrs's package manager):

```sh
zpm add MenkeTechnologies/zshrs-forgit
```

`zpm` clones the repo, runs `cargo build --release`, and `zmodload -R`s the
resulting `libforgit` — then `ga`, `glo`, … are live commands. To load it at
startup, add `zpm load forgit` to your `.zshrc`.

## Build manually

```sh
cargo build --release
zmodload -R ./target/release/libforgit.dylib   # .so on Linux
ga
```

## How it was ported

This is a worked example for the zshrs plugin porting guide:
[docs/PORTING_ZSH_PLUGIN.md](https://github.com/MenkeTechnologies/zshrs/blob/main/docs/PORTING_ZSH_PLUGIN.md).
Orchestration, argument handling, and sequencing are Rust; `git`/`fzf` run as
subprocesses; fzf `--preview` strings stay shell (fzf runs them via `sh`).

## License

MIT. Ported from [wfxr/forgit](https://github.com/wfxr/forgit) (MIT). See
[LICENSE](LICENSE).
