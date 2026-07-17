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

```sh
zpm load MenkeTechnologies/zshrs-forgit
```

Put that one line in your `.zshrc`.
[zpm](https://github.com/MenkeTechnologies/zshrs/blob/main/docs/ZPM.md),
zshrs's package manager, installs the plugin on the first shell start — clones
it, runs `cargo build --release`, and `zmodload -R`s the resulting `libforgit`
— then loads it from the store, zero-network, on every start after. No
separate install step; `ga`, `glo`, … are live commands.

### Manual build

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
