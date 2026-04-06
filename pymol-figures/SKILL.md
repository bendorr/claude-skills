---
name: pymol-figures
description: Generate publication-quality PyMOL figures of protein structures with ray tracing, transparent backgrounds, and automatic alignment for side-by-side comparisons.
user-invocable: true
allowed-tools: Read Bash Write Edit Grep Glob
argument-hint: [pdb_files...] [options]
---

# PyMOL Figure Generation

You are an expert at generating publication-quality protein structure figures using PyMOL. When the user asks you to make figures, follow these instructions precisely.

## General Workflow

1. **Load structures** — Load PDB/CIF files into PyMOL.
2. **Align structures** — If multiple structures are loaded, align them so they can be compared side by side.
3. **Style** — Apply the user's desired representation (cartoon, surface, sticks, etc.). Default to cartoon if not specified.
4. **Color** — Apply the user's desired coloring. If not specified, use distinct colors per structure.
5. **Ray trace settings** — Apply the standard ray trace settings below before saving.
6. **Save** — Ray trace and save as PNG with transparent background.

## Required Ray Trace Settings

Always apply these settings before saving any figure:

```python
set ray_trace_mode, 1
set ray_shadow, 0
set spec_reflect, 0
set spec_power, 0
set cartoon_loop_radius, 0.3
set ray_trace_gain, 0
set ambient, 0.3
set ray_trace_color, black
```

These settings produce clean, flat-shaded figures with black outlines and no specular highlights or shadows — ideal for publications and presentations.

## Transparent Background

Always set a transparent background before saving:

```python
set ray_opaque_background, 0
```

Save images as PNG to preserve transparency:

```python
png output.png, width=2400, height=2400, dpi=300, ray=1
```

Default to 2400x2400 at 300 DPI unless the user specifies otherwise.

## Aligning Multiple Structures

When making figures of multiple protein structures for side-by-side comparison, **always align them first** so the orientation is consistent:

```python
# Load structures
load structure1.pdb, struct1
load structure2.pdb, struct2

# Align struct2 onto struct1 (first loaded is reference)
align struct2, struct1
```

Use `align` for structures with similar sequences. Use `super` for structures with low sequence identity. Use `cealign` for structures with very different sequences or folds.

After alignment, **orient to the reference structure** so all images share the same view:

```python
orient struct1
```

Then save each structure individually by hiding/showing objects:

```python
# Save struct1
hide everything
show cartoon, struct1
ray
png struct1.png, width=2400, height=2400, dpi=300

# Save struct2
hide everything
show cartoon, struct2
ray
png struct2.png, width=2400, height=2400, dpi=300
```

**Do not re-orient or rotate between saves** — this ensures the images are directly comparable side by side.

## PyMOL Script Generation

When generating PyMOL scripts, write them as `.pml` files that can be run with:

```bash
pymol -cq script.pml
```

The `-c` flag runs PyMOL without a GUI (command-line mode), and `-q` suppresses the splash screen.

Structure every script as:

```python
# 1. Load structures
load input.pdb, my_structure

# 2. Align (if multiple structures)
align mobile, target

# 3. Set representation
hide everything
show cartoon

# 4. Set colors
color marine, struct1
color salmon, struct2

# 5. Orient the view
orient

# 6. Apply ray trace settings
set ray_trace_mode, 1
set ray_shadow, 0
set spec_reflect, 0
set spec_power, 0
set cartoon_loop_radius, 0.3
set ray_trace_gain, 0
set ambient, 0.3
set ray_trace_color, black
set ray_opaque_background, 0

# 7. Save
png output.png, width=2400, height=2400, dpi=300, ray=1
```

## Key Reminders

- Always use `ray=1` in the `png` command or call `ray` before `png` to ensure ray tracing is applied.
- Never change orientation between saving aligned structures.
- Use descriptive object names when loading (not default names).
- If the user provides a viewing angle or selection, respect it exactly.
- For detailed reference on ray trace settings, see [reference.md](reference.md).
- For example scripts, see [examples.md](examples.md).
