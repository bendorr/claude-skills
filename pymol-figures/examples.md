# Example PyMOL Scripts

## Single Structure — Cartoon

```python
# single_cartoon.pml
# Simple cartoon rendering of a single protein

load my_protein.pdb, protein

hide everything
show cartoon, protein
color marine, protein

orient

set ray_trace_mode, 1
set ray_shadow, 0
set spec_reflect, 0
set spec_power, 0
set cartoon_loop_radius, 0.3
set ray_trace_gain, 0
set ambient, 0.3
set ray_trace_color, black
set ray_opaque_background, 0

png protein_cartoon.png, width=2400, height=2400, dpi=300, ray=1
quit
```

## Two Aligned Structures — Side-by-Side Comparison

```python
# compare_structures.pml
# Align two structures and save individual images for side-by-side comparison

load wildtype.pdb, wt
load mutant.pdb, mut

# Align mutant onto wildtype
align mut, wt

# Set shared orientation based on reference
orient wt

# Style
hide everything
show cartoon

color marine, wt
color salmon, mut

# Ray trace settings
set ray_trace_mode, 1
set ray_shadow, 0
set spec_reflect, 0
set spec_power, 0
set cartoon_loop_radius, 0.3
set ray_trace_gain, 0
set ambient, 0.3
set ray_trace_color, black
set ray_opaque_background, 0

# Save wildtype
hide everything
show cartoon, wt
png wt_cartoon.png, width=2400, height=2400, dpi=300, ray=1

# Save mutant (same orientation — do NOT re-orient)
hide everything
show cartoon, mut
png mut_cartoon.png, width=2400, height=2400, dpi=300, ray=1

quit
```

## Active Site Detail with Sticks

```python
# active_site.pml
# Cartoon with active site residues shown as sticks

load enzyme.pdb, enzyme

hide everything
show cartoon, enzyme
color palecyan, enzyme

# Show active site residues as sticks
show sticks, enzyme and resi 45+72+110+145
color atomic, enzyme and resi 45+72+110+145
util.cnc enzyme and resi 45+72+110+145  # color by element, keep carbons

# Zoom into active site
center enzyme and resi 45+72+110+145
zoom enzyme and resi 45+72+110+145, 8

set ray_trace_mode, 1
set ray_shadow, 0
set spec_reflect, 0
set spec_power, 0
set cartoon_loop_radius, 0.3
set ray_trace_gain, 0
set ambient, 0.3
set ray_trace_color, black
set ray_opaque_background, 0

png active_site.png, width=2400, height=2400, dpi=300, ray=1
quit
```

## Multiple Structures — Batch Alignment and Export

```python
# batch_compare.pml
# Align and export multiple structures for comparison

load reference.pdb, ref
load model_1.pdb, m1
load model_2.pdb, m2
load model_3.pdb, m3

# Align all to reference
align m1, ref
align m2, ref
align m3, ref

# Orient to reference
orient ref

# Ray trace settings
set ray_trace_mode, 1
set ray_shadow, 0
set spec_reflect, 0
set spec_power, 0
set cartoon_loop_radius, 0.3
set ray_trace_gain, 0
set ambient, 0.3
set ray_trace_color, black
set ray_opaque_background, 0

# Define colors
color marine, ref
color salmon, m1
color splitpea, m2
color lightorange, m3

# Save each individually
foreach obj in [ref, m1, m2, m3]: hide everything; show cartoon, obj; png f"{obj}_cartoon.png", width=2400, height=2400, dpi=300, ray=1

quit
```

Note: The `foreach` loop syntax works in PyMOL 2.x+. For older versions, write out each hide/show/png block explicitly.

## Overlay — All Structures in One Image

```python
# overlay.pml
# Show all aligned structures in a single image

load reference.pdb, ref
load model_1.pdb, m1
load model_2.pdb, m2

align m1, ref
align m2, ref

hide everything
show cartoon

color marine, ref
color salmon, m1
color splitpea, m2

orient ref

set ray_trace_mode, 1
set ray_shadow, 0
set spec_reflect, 0
set spec_power, 0
set cartoon_loop_radius, 0.3
set ray_trace_gain, 0
set ambient, 0.3
set ray_trace_color, black
set ray_opaque_background, 0

png overlay.png, width=2400, height=2400, dpi=300, ray=1
quit
```
