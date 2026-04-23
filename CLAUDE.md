# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **LaTeX academic paper** for PLNTP (Pulse Localization Network with Temporal Prior) — a deep learning system that localizes three TCM pulse points (寸/cun, 關/guan, 尺/chi) on the wrist from infrared video, using a U-Net architecture with a Temporal Attention Prior mechanism.

## Build Commands

```bash
# Full build (run pdflatex twice to resolve cross-references)
pdflatex main.tex && bibtex main && pdflatex main.tex && pdflatex main.tex

# Quick draft (single pass, faster)
pdflatex main.tex
```

## Repository Structure

- `main.tex` — Single-file paper (abstract, introduction, related work, method, experiments, conclusion)
- `reference.bib` — BibTeX bibliography (~30 references)
- `figures/` — Figure assets referenced in the paper

## Architecture Described in the Paper

The paper describes (not implements) the following:

- **Input**: Grayscale infrared video tensor (90 frames × 136 × 256 px)
- **Backbone**: U-Net encoder-decoder with skip connections
- **Temporal Attention Prior**: Computed from frame-wise absolute differences summed across 90 frames, thresholded to highlight pulse-motion regions (Equations 1–5 in §3)
- **Fusion**: Prior is injected at the **decoder side via concatenation** after a convolution (best-performing among ablated strategies)
- **Output**: Three heatmaps → converted to keypoint coordinates via Circular Region Expectation (weighted center-of-mass within a circular mask)

## Key Experimental Details

- Dataset: 2,949 infrared videos, split 7:2:1 (train:val:test)
- Optimizer: RMSprop, LR 1×10⁻⁵, halved every 10 epochs, 100 epochs total, batch size 4
- Hardware: RTX 3090
- Evaluation metric: PCK (Percentage of Correct Keypoints) at distance thresholds of 20/40/60/80/100 px
