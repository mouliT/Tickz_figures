# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Purpose

LaTeX/TikZ/CircuiTikZ repository for paper-ready technical figures: power electronics circuits, electrical circuits, block diagrams, timing diagrams, plots. Target quality: journal/conference publication (IEEE, Elsevier).

## Workflow — 3-Iteration Rule

**Every figure must be done in ≤ 3 compile-feedback cycles.**

```
Iteration 1 — Topology skeleton
  Draw: named coordinates + wires only. No component symbols yet.
  User confirms: connectivity is correct.

Iteration 2 — Full circuit
  Add: component symbols, labels, current/voltage arrows.
  User confirms: symbols correct, layout acceptable.

Iteration 3 — Polish
  Fix: spacing, font sizes, line weights, label offsets.
  Output: paper-ready.
```

If iteration 3 is not final, something is wrong with the approach — restart with a cleaner coordinate grid.

---

## Reference Style (Golden Standard)

All figures must match the style of `Example_files/Buck.tex`, `Boost.tex`, `Buck_boost.tex`, `Electrical_model_superCaps.tex` by Amir Ostadrahimi. Study these files before drawing anything new.

---

## Mandatory Document Header

**Every circuit figure must use this exact header — no deviations:**

```latex
\documentclass[border=5pt]{standalone}
\usepackage{tikz}
\usepackage[american,cuteinductors,smartlabels]{circuitikz}

% Component dimensions
\ctikzset{bipoles/thickness=0.7}
\ctikzset{bipoles/length=1.5cm}
\ctikzset{bipoles/resistor/width=0.7}
\ctikzset{bipoles/resistor/height=0.25}
\ctikzset{bipoles/diode/height=0.3}
\ctikzset{bipoles/diode/width=0.3}
\ctikzset{tripoles/thickness=0.7}
\ctikzset{bipoles/battery1/height=0.7}

% Global styles
\tikzstyle{every node}=[font=\Large]
\tikzstyle{every path}=[line width=0.9pt, line cap=round, line join=round]
```

Key reasons these options matter:
- `american` — flat resistor boxes, American voltage source symbol
- `cuteinductors` — sine-wave coil inductor (not plain bumps)
- `smartlabels` — automatic label placement avoids collisions
- `line cap=round, line join=round` — professional wire corners

---

## CircuiTikZ Coding Rules (Derived from Example Files)

### Rule 1: Always use `\begin{circuitikz}`, not `\begin{tikzpicture}`
```latex
\begin{document}
  \begin{circuitikz}
    ...
  \end{circuitikz}
\end{document}
```

### Rule 2: Coordinate-first — name every key node before drawing
```latex
\coordinate (VBottom) at (0,0);
\draw (VBottom) to[battery1, l=$V_{in}$, invert] ++(0,3) coordinate (VTop);
\draw (VTop) to[short] ++(1,0) to[L, l=$L$] ++(2,0) coordinate (InductorRight);
```
All coordinates get descriptive names: `(VTop)`, `(VBottom)`, `(DiodeTop)`, `(InductorRight)`, etc.

### Rule 3: Use `++()` relative coordinates always
```latex
to[short] ++(0.8,0)   % move right 0.8
to[L, l=$L$] ++(2,0)  % inductor 2 units right
++(0,-0.8)            % move down 0.8
```

### Rule 4: Intersection coordinates `(A|-B)` for vertical drops
```latex
% From DiodeTop, drop straight down to the level of VBottom:
\draw (DiodeTop) to[D*, invert] (DiodeTop|-VBottom);

% From CapacitorTop, drop to ground rail:
\draw (CapacitorTop) to[C, l_=$C$] (CapacitorTop|-VBottom);
```
This is the standard way to connect vertical branches to the ground rail.

### Rule 5: NEVER use `to[Tnmos]` or `to[Tpmos]` inline for transistors
**WRONG (causes broken symbol rendering):**
```latex
\draw (1,3) to[Tnmos, n=Q1] (1,1.5);  % DO NOT USE
```

**CORRECT — use `\node` with `anchor=C` and `rotate`:**
```latex
% Horizontal IGBT/MOSFET (switch in top rail)
\draw (VTop) to[short] ++(0.8,0)
  node[nigbt, anchor=C, rotate=90,
       label={[yshift=-1.5cm] \normalsize $Q_1$}] (Q1) {};

% Continue from emitter/source:
\draw (Q1.E) to[short] ++(0.5,0) coordinate (SwNode);

% Gate connection:
\draw (Q1.B) -- ++(0,0.8) node[above] {$g$};
```

Available tripole names: `nigbt`, `pigbt`, `nmos`, `pmos`, `npn`, `pnp`
Anchors: `.C` (collector/drain), `.E` (emitter/source), `.B` (base/gate)
Use `rotate=90` for horizontal placement, `rotate=-90` for opposite orientation.

### Rule 6: Voltage source always uses `invert`
```latex
% + terminal ends up at the TOP (VTop) — invert is required
\draw (VBottom) to[battery1, l=$V_{in}$, invert] ++(0,3) coordinate (VTop);
\draw (VBottom) to[V,        l=$V_{in}$, invert] ++(0,3) coordinate (VTop);
```
Without `invert`, `+` appears at the bottom (the `from` node). **Always use `invert` when drawing a source upward.**

### Rule 7: Diode orientation — use `D*` for filled (solid) diodes
```latex
% Freewheeling diode drawn DOWNWARD but conducting UPWARD (cathode at top):
\draw (TopNode) to[D*, invert] ++(0,-1.5) coordinate (DNode);
%   invert on downward draw → cathode at top (TopNode), anode at bottom (DNode) ✓

% Series diode conducting DOWNWARD (anode at top, cathode at bottom):
\draw (TopNode) to[D*] ++(0,-1.5) coordinate (DNode);
%   no invert on downward draw → anode at top, cathode at bottom ✓
```
`D*` = filled black diode (preferred for power electronics). `D` = outline diode.

### Rule 7b: Vertical inductor label placement — avoid same-side clutter
```latex
% WRONG: both labels on same side — cluttered
\draw (A) to[L, l_=$L$, i>_=$i_L$] (B);

% CORRECT: labels on opposite sides
\draw (A) to[L, l_=$L$, i>^=$i_L$] (B);  % L right, iL left
```

### Rule 8: Labels — use `l_=` (below), `l^=` (above), `v^=`, `i>_=`
```latex
to[L, l_=$L$, v^=$v_L$, i>_=$i_L$]   % label below, V above, I below
to[C, l_=$C$, i>^=$i_c$]              % label below, I above
to[R, l_=$R$, v^=$V_{out}$]           % label below, V above
```
For vertical components (capacitors, diodes to ground), `l_=` puts the label to the right.

### Rule 9: Use `\newcommand` for repeated dimensions
```latex
\newcommand\gap{0.2}
\newcommand\Rlength{2.1}
\newcommand\Clength{2.9}
```
This makes spacing adjustments in one place.

### Rule 10: Conversion ratio and annotations as `\coordinate[label=...]`
```latex
\coordinate[label={[xshift=0,yshift=0] \large $M(D)=\frac{V_{out}}{V_{in}}=D$}]
  (M) at (3,-1);
```

---

## PGFPlots Pattern (for M(D) curves and waveforms)

```latex
\def\xo{2}   % axis origin x
\def\yo{-7}  % axis origin y
\def\length{5}
\def\N{200}

\draw[->] (\xo-0.2, \yo) --++(\length,0) node[right] {\small $D$};
\draw[->] (\xo, \yo-0.2) --++(0,\length)  node[above] {\small $M(D)$};
\draw plot[samples=\N, domain=0:4, xshift=\xo cm, yshift=\yo cm] (\x, \x);
```

---

## Common Mistakes to Avoid

| Mistake | Correct approach |
|---|---|
| `to[Tnmos]` inline | `\node[nigbt, anchor=C, rotate=90]` |
| `\begin{tikzpicture}` for circuits | `\begin{circuitikz}` |
| `\usepackage{circuitikz}` without options | `\usepackage[american,cuteinductors,smartlabels]{circuitikz}` |
| Missing `invert` on upward voltage source | Always add `invert` |
| `l2_=` | Does not exist — use `l_=` |
| Raw coordinates `(1,2)` without names | Name all key nodes with `\coordinate` |
| Two labels on one component in tight layout | Use `\node` offset separately |
| `\documentclass[tikz,border=4pt]{standalone}` | Use `\documentclass[border=5pt]{standalone}` + separate `\usepackage{tikz}` |

---

## Multi-figure Documents

For figures with multiple sub-diagrams (like `Electrical_model_superCaps.tex`), use multiple `\begin{tikzpicture}...\end{tikzpicture}` blocks inside one `\begin{document}`. Each block stacks vertically in the standalone output.

---

## Git & Overleaf Workflow

1. Write/edit `.tex` in this repo
2. `git push origin main`
3. User pulls in Overleaf (GitHub sync), compiles, screenshots result
4. Feedback → fix → repeat (max 3 iterations per figure)
