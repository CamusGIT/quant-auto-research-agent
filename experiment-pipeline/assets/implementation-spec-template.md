# Implementation Specification: [Method Name]

> Fill this template when entering From-Scratch mode (no usable baseline source code).
> Read the anchor paper at L1 level via `local-paper-navigator`'s `fetch_paper.py --reading-level L1` and extract each section.

## Algorithm Core

- **Key equations**: [list all key equations with variable definitions]
- **Pseudocode**: [convert each equation to step-by-step pseudocode]
- **Data flow**: [input → intermediate states → output diagram]

## Architecture Details

- **State space**: [definition, transitions, terminal states — or [IMPLICIT]]
- **Policy networks**: [structure, hidden dims, activation — or [IMPLICIT]]
- **Other components**: [list all architecture components; mark [IMPLICIT] for anything the paper doesn't specify]

## Data Pipeline

- **Feature definitions**: [complete list of all features with formulas]
- **Preprocessing order**: [exact sequence: step1 → step2 → step3, with parameters]
- **Expected statistics**: [paper-reported mean/std/IC for verification]

## Training Specification

- **Loss function**: [exact formula with variable definitions]
- **Optimizer + schedule**: [type, learning rate, schedule]
- **Batch construction**: [trajectory sampling strategy — or [IMPLICIT]]
- **Training duration**: [epochs/steps, early stopping criteria]

## [IMPLICIT] Markers Summary

| # | Detail | Inference Basis | Resolution Level |
|---|--------|-----------------|-----------------|
| 1 | [detail paper omits] | [how you infer it] | L1/L2/L3/L4 |
| 2 | ... | ... | ... |
