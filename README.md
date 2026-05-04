# Theme sources

YAML theme trees for [ThemeWeaver](../README.md). Each **subdirectory** of `themes/` is one theme (`README.md` and `__init__.py` are not themes). The folder name is the theme id used by the CLI (`export`, `validate`, etc.).

## Catalog

| Theme | Description |
| ----- | ----------- |
| **Brutalism** (`brutalism`) | Brutalism palette inspired in the brutalist architecture |
| **Catppuccin Mocha** (`catppuccin-mocha`) | A personal version of the Catppuccin theme, Mocha variant |
| **Dracula** (`dracula`) | Based on the Dracula color palette by Zeno Rocha |
| **Gruvbox** (`gruvbox`) | A personal version of the classic Gruvbox theme |
| **IDLE** (`idle`) | A personal version of the default theme from the famous Idle editor |
| **Inkpot** (`inkpot`) | A port of the Vim InkPot theme |
| **Miami Nights** (`miami-nights`) | Synthwave / Miami Nights palette from JupyterLab Miami Nights (timkpaine) |
| **Monokai** (`monokai`) | A personal version of the popular Monokai theme |
| **Notepad++** (`notepad++`) | A personal version of the default theme from the Notepad++ editor, originally from Eclipse |
| **Obsidian** (`obsidian`) | A personal version of the Obsidian theme from Eclipse |
| **Solarized** (`solarized`) | Based on the Solarized color palette by Ethan Schoonover, enhanced with a wider range of colors |
| **Spyder** (`spyder`) | Based on the default QDarkStyleSheet color palette |
| **Zenburn** (`zenburn`) | A low-contrast color scheme easy for your eyes and designed to keep you in the zone for long programming sessions |

## Working with ThemeWeaver

Run all `pixi` / CLI commands from the **ThemeWeaver repository root** (the directory that contains `pyproject.toml` and this `themes/` folder). The tools resolve `themes/` relative to the current working directory unless you pass `--theme-dir`.

### Setup

- Install [Pixi](https://pixi.sh/) and run `pixi install` in the repo root.
- Python and dependencies are provided by the Pixi environment (see the main [README](../README.md#project-setup)).

### Create a new theme (`generate`)

`generate` writes `theme.yaml`, `colorsystem.yaml`, and `mappings.yaml` under `themes/<name>/`. It does **not** write to `build/`; use `export` after you are satisfied.

You need **six seed colors** in order: primary, secondary, error, success, warning, group (see the table in [Theme generation](../README.md#theme-generation) in the main README). Each color must be `#RRGGBB` (six hex digits; shorthand `#RGB` is not accepted).

Minimal example:

```bash
pixi run generate my_theme \
  --colors "#1e1e2e" "#b4befe" "#f38ba8" "#a6e3a1" "#fab387" "#eba0ac" \
  --display-name "My Custom Theme"
```

Other inputs:

- **YAML definition:** `pixi run generate my_theme --from-yaml theme-definition.yaml` (see [From a YAML definition file](../README.md#from-a-yaml-definition-file)).
- **Syntax seeds or full 17-color syntax lists, editor bold/italic:** `--syntax-colors-dark`, `--syntax-colors-light`, `--syntax-format`, or the equivalent fields in the YAML definition.

Useful flags: `--overwrite`, `--output-dir <path>` (if not `./themes`, pass `--theme-dir` to `export` / `validate`), `--variants dark light`, `--no-validate-contrast`.

Full detail: [Theme generation](../README.md#theme-generation), [Syntax colors](../README.md#syntax-colors), [Syntax format](../README.md#syntax-format).

### Validate and export

```bash
pixi run validate my_theme
pixi run validate-contrast my_theme
pixi run export my_theme          # dark + light into build/
pixi run export-dark my_theme     # dark only
pixi run export-light my_theme    # light only
```

`validate` checks YAML structure; `validate-contrast` checks colors against bundled Spyder UI rules. Export uses QDarkStyle and writes assets under `build/`.

**Shell quoting:** If the theme id contains characters the shell treats specially (e.g. `notepad++`), quote it:

```bash
pixi run export 'notepad++'
```

### Edit an existing theme

You may hand-edit `colorsystem.yaml`, `mappings.yaml`, and `theme.yaml` after generation or as a fully manual theme. A heavily commented reference for syntax slot layout is `catppuccin-mocha/colorsystem.yaml` (`CatppuccinMochaSyntaxDark` / `CatppuccinMochaSyntaxLight`). After edits, run `validate` / `validate-contrast` and `export` again.

### List themes and metadata

```bash
pixi run list-themes
pixi run theme-info solarized
```

The underlying CLI subcommand for listing is `list` (`pixi run cli list`).

### Preview and packaging

- **Preview** (Qt): export first, then `pixi run preview` from the repo root (see [Quick start](../README.md#quick-start)).
- **Spyder wheel/sdist:** `export` or `export-all`, then `pixi run package` (see [Spyder Python package](../README.md#spyder-python-package-python-package)).

### Color helpers (optional)

Palette, gradient, and interpolation commands are documented under [Color utilities](../README.md#color-utilities). They print to stdout and do not modify `themes/` unless you copy values yourself.
