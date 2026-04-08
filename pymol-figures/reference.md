# Ray Trace Settings Reference

## Setting Descriptions

| Setting | Value | Purpose |
|---------|-------|---------|
| `ray_trace_mode` | `1` | Quantized ray tracing — produces clean black outlines around structures |
| `ray_shadow` | `0` | Disables shadows for a flat, diagram-like appearance |
| `spec_reflect` | `0` | Removes specular reflections (no shiny highlights) |
| `spec_power` | `0` | Eliminates specular power (reinforces no-shine look) |
| `cartoon_loop_radius` | `0.3` | Sets loop tube radius for cartoon representation |
| `ray_trace_gain` | `0` | Controls outline darkness; 0 gives clean outlines without over-darkening |
| `ambient` | `0.3` | Base ambient lighting; 0.3 gives even illumination without washing out color |
| `ray_trace_color` | `black` | Color of the ray trace outlines |
| `cartoon_side_chain_helper` | `1` | Connects stick side chains back to their cartoon backbone (prevents floating sticks) |
| `ray_opaque_background` | `0` | Transparent background (essential for compositing figures) |

## Alignment Commands

| Command | Use When |
|---------|----------|
| `align mobile, target` | Structures share high sequence identity (>30%) |
| `super mobile, target` | Structures have low sequence identity but similar fold |
| `cealign target, mobile` | Structures have very different sequences or folds. Note: argument order is reversed compared to `align`/`super` |

## Sidechain Display Settings

| Setting / Command | Value | Purpose |
|-------------------|-------|---------|
| `cartoon_side_chain_helper` | `1` | Connects stick sidechains to the cartoon backbone with a visible stub from the CA atom |
| `remove hydrogens` | — | Removes hydrogen atoms for cleaner sidechain rendering |
| `util.cnc selection` | — | Colors heteroatoms by element (N=blue, O=red, S=yellow, etc.) while preserving the existing carbon color |

## Common Representations

| Representation | Command | Best For |
|----------------|---------|----------|
| Cartoon | `show cartoon` | Overall fold, secondary structure |
| Sticks | `show sticks, resi 100-110` | Active site residues, ligand interactions |
| Surface | `show surface` | Shape complementarity, binding interfaces |
| Spheres | `show spheres` | Space-filling models, atom-level detail |
| Mesh | `show mesh` | Surface with underlying structure visible |

## Side Chain Carbon Coloring Convention

When showing residues as sticks, **keep carbon atoms the same color as their chain's cartoon backbone**. Use `util.cnc` (color by element, preserving carbon color) instead of `color atomic` or `util.cbag`:

```python
# Correct: carbons stay marine (matching chain A cartoon), heteroatoms colored by element
color marine, chain A
show sticks, chain A and resi 100+200
util.cnc chain A and resi 100+200

# Wrong: do NOT recolor carbons to a different uniform color
# color atomic, chain A and resi 100+200    # makes all carbons gray
# util.cbag chain A and resi 100+200        # makes carbons green
```

This makes it visually clear which chain each side chain belongs to.

## Useful Color Schemes

```python
# Color by chain
util.cbc          # Color by chain, automatic distinct colors

# Color by secondary structure
color red, ss h   # Helices
color yellow, ss s  # Sheets
color green, ss l+''  # Loops

# Color by B-factor
spectrum b, blue_white_red

# Color by residue position
spectrum count, rainbow
```

## Image Size Guidelines

| Use Case | Width x Height | DPI |
|----------|---------------|-----|
| Journal figure (single column) | 2400 x 2400 | 300 |
| Journal figure (double column) | 4800 x 2400 | 300 |
| Presentation slide | 3840 x 2160 | 150 |
| Poster panel | 4800 x 4800 | 300 |
