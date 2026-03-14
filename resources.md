# Resources for TikZ / CircuiTikZ Figure Drawing

## Local Reference Files (Golden Standard)

| File | What it demonstrates |
|---|---|
| `Example_files/Buck.tex` | Buck converter with IGBT, `D*`, `battery1`, voltage/current labels, M(D) plot |
| `Example_files/Boost.tex` | Boost converter, vertical IGBT placement, `(A\|-B)` intersections |
| `Example_files/Buck_boost.tex` | Vertical inductor, combined topology |
| `Example_files/Electrical_model_superCaps.tex` | Multi-tikzpicture layout, `\newcommand` dimensions, `vC`, dashed lines, filled rectangles |
| `Example_files/pgfmanual.pdf` | PGF/TikZ full manual (~1100 pages), authoritative reference |

**Author of example files: Amir Ostadrahimi** — style these examples define is the target quality.

---

## Online Resources

### Primary
| Resource | URL | Use for |
|---|---|---|
| CircuiTikZ manual (latest) | https://mirrors.ctan.org/graphics/pgf/contrib/circuitikz/circuitikzmanual.pdf | Component names, options, anchors |
| PGF/TikZ online manual | https://tikz.dev/ | All TikZ primitives, coordinate systems, math |
| tikz.org | https://tikz.org/ | Book examples, online compiler |

### GitHub Repositories
| Repo | URL | Use for |
|---|---|---|
| LaTeX Graphics with TikZ (Packt) | https://github.com/PacktPublishing/LaTeX-graphics-with-TikZ | 15-chapter worked examples |
| PGF official source | https://github.com/pgf-tikz/pgf | Source + full manual TeX |

### Chapter map for PacktPublishing/LaTeX-graphics-with-TikZ
- Ch03: Nodes, positioning, alignment → block diagrams
- Ch04: Edges and arrows → signal flow graphs
- Ch07: Filling, clipping, shading → highlighted regions
- Ch08: Decorating paths → brace annotations, zigzag
- Ch10: Coordinate calculations → `calc` library, `let` syntax
- Ch13: Plotting 2D/3D → PGFPlots, Bode, waveforms
- Ch14: Diagrams → flowcharts, system diagrams

---

## Key Package Versions (Overleaf current)
- CircuiTikZ: 1.6.x (use `\usepackage[american,cuteinductors,smartlabels]{circuitikz}`)
- PGFPlots: 1.18 (use `\pgfplotsset{compat=1.18}`)
- siunitx: v3 (syntax unchanged from v2 for basic use)
