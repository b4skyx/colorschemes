# clearsight

Colorschemes for **red-green colour blindness (strong deutan)**. Each one was optimised against a vision model
that was *measured*, not guessed. Every syntax role, diagnostic, git sign and ANSI colour is placed where a
deuteranope can still tell it apart, while keeping a calm, good-looking palette for everyone else.

| Scheme | Character | Text contrast | Syntax distinctions kept¹ | Closest core pair² | Error vs warning² | Added vs deleted² | Terminal worst pair² |
|---|---|---|---|---|---|---|---|
| **Haze** | dark, muted, low-contrast, warm-grey | 7.1:1 | 100% | 14.4 | 24.9 | 44.8 | 19.4 |
| **Fog** | dark, muted, low-contrast, cool-grey | 7.0:1 | 100% | 11.9 | 24.9 | 43.5 | 18.7 |
| **Abyss** | dark, near-black, oled, high-clarity | 11.2:1 | 98% | 16.9 | 25.1 | 62.5 | 20.6 |
| **Dusk** | dark, soft, violet, rose-pine-like | 11.5:1 | 100% | 14.7 | 25.0 | 51.4 | 20.4 |

¹ Share of the role pairs the scheme intends to differ that still differ in deutan vision.
² CIEDE2000 between *simulated* colours (≥ 15 comfortable, 10–15 fine, < 5 easily confused).
For comparison, the best ready-made editor themes reach 6.9 (Rosé Pine) and 8.6 (Terafox) for the closest core pair,
and none of 819 tested terminal themes reaches 18 without neon colours. See [docs/vision-model.md](docs/vision-model.md).

Light counterparts (Haze Light, Fog Light, Abyss Light, Dusk Light) are in progress.

## Layout

```
haze/ fog/ abyss/ dusk/
  colorschemes.json   tagged palette, semantic roles, terminal colours, full Neovim highlight map, scores
  alacritty.toml      Alacritty colours
  kitty.conf          kitty colours
colors/ lua/          Neovim plugin (coming soon; installable with lazy.nvim)
tools/                colour maths and generators (Python 3, no dependencies)
docs/                 vision model and scoring method
```

## Install

**Alacritty**: in `~/.config/alacritty/alacritty.toml`:
```toml
[general]
import = ["~/path/to/clearsight/haze/alacritty.toml"]
```

**kitty**: in `~/.config/kitty/kitty.conf`:
```
include ~/path/to/clearsight/haze/kitty.conf
```

**Neovim**: coming soon:
```lua
{ "<you>/clearsight", lazy = false, priority = 1000, config = function() vim.cmd.colorscheme("haze") end }
```

## `colorschemes.json`

| Key | Contents |
|---|---|
| `id`, `name`, `variant`, `tags`, `description` | metadata (`tags` e.g. `muted`, `low-contrast`, `deutan-optimised`) |
| `palette.<name>` | `hex`, `oklch` and `tags` listing every place the colour is used (`syntax.keyword`, `terminal.blue`, `diagnostic.error`, …) |
| `terminal` | background, foreground, cursor, selection and the 16 ANSI colours (`normal` / `bright`) |
| `syntax_styles` | italic/bold per role |
| `highlights.<group>` | Neovim highlight groups incl. treesitter captures; each colour has `hex` and the palette `ref` |
| `scores` | editor and terminal separation scores for the target vision |

## Regenerate

```sh
python3 tools/gen_schemes.py --only haze,fog,abyss,dusk   # optimise editor roles -> tools/build/custom_schemes.json
python3 tools/export_schemes.py                           # optimise terminal colours + write the scheme folders
```
Styles (background, foreground, hue anchors, limits) are defined at the top of `tools/gen_schemes.py`.

## License
MIT
