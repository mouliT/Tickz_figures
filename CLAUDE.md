# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Purpose

This is a LaTeX/TikZ figure library for creating paper-ready technical figures: power electronics circuits, electrical circuits, block diagrams, timing diagrams, and plots. All figures target journal/conference publication quality.

## Workflow

1. Write/edit `.tex` files here
2. Push to GitHub (`https://github.com/mouliT/Tickz_figures`)
3. User pulls into Overleaf via GitHub sync, compiles, and gives visual feedback
4. Iterate based on feedback

## Repository Structure

```
circuits/
  power_electronics/   # Converters, inverters, MOSFETs, IGBTs, gate drives
  analog/              # Op-amps, filters, amplifiers
plots/                 # PGFPlots data plots and graphs
block_diagrams/        # Control loops, system architecture
timing/                # Timing/waveform diagrams
templates/             # Reusable base templates
```

Each figure is a **standalone compilable file** using `\documentclass[tikz,border=4pt]{standalone}`.

## Core LaTeX Packages

| Package | Purpose |
|---|---|
| `circuitikz` | Circuit elements — resistors, caps, MOSFETs, diodes, sources, op-amps |
| `tikz` | Core drawing engine |
| `pgfplots` | Data plots, Bode plots, waveforms |
| `siunitx` | SI units: `\SI{100}{\micro\henry}`, `\SI{48}{\volt}` |
| `amsmath` | Math in labels |
| `xcolor` | Colors |

Key TikZ libraries to load: `arrows.meta`, `positioning`, `calc`, `shapes.geometric`, `decorations.pathreplacing`, `patterns`

## CircuiTikZ Reference (Critical)

### Component syntax
```latex
\draw (x1,y1) to[component, options] (x2,y2);
```

### Power Electronics Components
```latex
to[short]                   % wire
to[R, l=$R_1$]             % resistor
to[C, l=$C_1$]             % capacitor
to[L, l=$L_1$]             % inductor
to[V, v=$V_s$]             % voltage source
to[I, i=$I_s$]             % current source
to[D, l=$D_1$]             % diode (anode→cathode direction of draw)
to[zD]                      % zener diode
to[Tnmos, l=$Q_1$]         % N-channel MOSFET
to[Tpmos]                  % P-channel MOSFET
to[nigbt]                  % N-IGBT
to[pigbt]                  % P-IGBT
to[switch]                 % ideal switch
to[battery1]               % battery
to[transformer]            % transformer
to[cute inductor]          % inductor (alternative style)
```

### Node placement for MOSFETs/IGBTs (named nodes)
```latex
\node[nigfete] (Q1) at (2,2) {};
\draw (Q1.drain) -- ++(0,0.5);
\draw (Q1.source) -- ++(0,-0.5);
\draw (Q1.gate) -- ++(-0.5,0);
```

### Ground and supply rails
```latex
\draw (node) node[ground] {};
\draw (node) node[vcc] {$V_{DD}$};
\draw (node) node[vee] {};
```

### Labeling
```latex
to[R, l=$R_1$, l2_=$\SI{10}{\kilo\ohm}$]   % label + value below
to[V, v=$V_{in}$]                            % voltage label (with arrow)
to[R, i>=$i_L$]                              % current direction label
```

## Figure Quality Standards

- Use `\documentclass[tikz,border=4pt]{standalone}` for all figures
- Font: match target journal — default Computer Modern is fine for IEEE
- Line widths: `thick` (0.8pt) for main paths, `very thick` (1.2pt) for emphasis
- Colors: use `black` or minimal color; if color needed use `blue!70!black`, `red!70!black`
- Arrow tips: `{Latex[scale=0.8]}` for clean arrows
- Node labels: use `$math$` mode always for electrical quantities
- Coordinate system: prefer named coordinates `(n1)`, `(n2)` over raw numbers for maintainability

## Overleaf Compatibility Notes

- No shell-escape needed for pure TikZ/CircuiTikZ
- `pgfplots` with `compat=1.18` recommended
- Avoid absolute file paths; all `\input` paths relative to root

## Common Pitfalls with CircuiTikZ

- Diode direction: current flows in direction of `\draw` for `to[D]` (anode to cathode = left to right if drawing left→right)
- MOSFET drain/source: in `Tnmos`, drain is top, source is bottom by default
- Voltage arrows `v=` point from − to + by default; use `v^=` or `v<=` to flip
- Ground nodes must be at wire endpoints, not mid-wire
- Use `\ctikzset{voltage/distance from node=.5}` to adjust label spacing globally
