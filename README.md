# CLARITY  
**State-Conditioned Analog Retrieval for ICU Time-Series**

CLARITY is an end-to-end machine learning system for **analogical reasoning over ICU physiological time-series**.  
Given a short window of recent vitals, the system retrieves historically similar ICU trajectories, aligns them in time, and visualizes how those situations evolved—while exposing *what features are driving similarity right now*.

CLARITY does **not** predict outcomes, assign diagnoses, or recommend treatments.  
Instead, it organizes historical experience to support reasoning under uncertainty.

---

## Why CLARITY?

Clinical reasoning is often analogical:  
*“Have I seen something like this before, and what happened next?”*

Most ML systems in healthcare collapse this question into a single scalar prediction (e.g., risk or mortality), obscuring uncertainty and relying on noisy labels. CLARITY takes a different approach:

- Learn representations directly from multivariate vitals
- Define similarity in a learned latent space
- Retrieve and visualize *full historical trajectories*
- Reveal the model’s internal reasoning via attention and latent modes

The result is a system that supports exploration, comparison, and interpretation—without claiming authority.

---

## What the System Does

Given a recent window of ICU vitals:

1. **Encodes** the window into a latent representation using a temporal encoder with self-attention.
2. **Infers a latent physiological state**, represented as a soft mixture over anonymous latent modes.
3. **Retrieves analog trajectories** from a large ICU dataset based on learned similarity.
4. **Aligns and visualizes** retrieved cases alongside their future evolution.
5. **Exposes model reasoning** by showing:
   - latent mode composition
   - attention over vitals and time indicating what is contributing to similarity *right now*.

As new data arrives, the inferred state and retrieved neighbors evolve, enabling inspection of **state drift** over time.

---

## What the System Explicitly Does *Not* Do

- ❌ No diagnosis prediction  
- ❌ No treatment recommendation  
- ❌ No mortality or risk scoring  
- ❌ No claim of clinical decision authority  

Diagnoses, treatments, and outcomes are used **only post hoc** as annotations or evaluation probes.

---

## Core Idea

Attention in CLARITY is **state-conditioned, not label-conditioned**.

The model does *not* say:
> “Because the disease is X, attend to vital Y.”

Instead, it learns:
> “Given the latent state inferred from the data, these interactions matter more right now.”

Latent states may later align with named diseases, may remain ambiguous, or may drift over time.

---

## Model Overview

- **Input:**  
  Fixed-length windows of multivariate ICU vitals  
  (e.g., HR, BP, SpO₂, RR at 5-minute resolution)

- **Encoder:**  
  Shared per-token encoder + temporal self-attention

- **Latent Modes:**  
  A mixture-of-experts mechanism infers a soft distribution over anonymous latent modes, allowing different physiological interaction patterns to emerge without labels.

- **Embedding:**  
  Each window is summarized into a single vector representing the current latent physiological state.

- **Training:**  
  Self-supervised temporal objectives (e.g., future latent alignment, overlapping window consistency).  
  Prediction occurs during training only; inference uses retrieval.

- **Retrieval:**  
  Nearest-neighbor search in latent space over historical ICU windows.

---

## Interactive Demo

The demo allows users to:

1. Select a preloaded ICU vitals window (or upload one).
2. Visualize the query trajectory.
3. Retrieve the most similar historical trajectories.
4. Compare how those cases evolved after the matched window.
5. Inspect:
   - inferred latent mode composition
   - attention maps showing which vitals and time segments drive similarity.
6. Optionally slide forward in time to observe how similarity and latent state drift.

This demonstrates the full system end-to-end: perception → memory → interpretation.

---

## Evaluation

Evaluation focuses on representation quality rather than prediction accuracy:

- **Future coherence:**  
  Do retrieved neighbors exhibit more similar future trajectories than baselines?

- **Neighborhood enrichment:**  
  Do diagnoses or outcomes cluster meaningfully *post hoc* in latent neighborhoods?

- **Temporal sensitivity:**  
  Do neighborhoods shift earlier than overt deterioration?

Baselines include raw kNN on vitals, DTW-based similarity, and encoders without latent modes.

---

## Datasets

CLARITY is designed for public ICU datasets with dense vitals, such as:

- eICU-CRD
- HiRID
- MIMIC-IV (clinical or waveform variants)

Dataset-specific preprocessing scripts are provided separately.

---

## Repository Structure
clarity/
README.md
SPEC.tex
src/
data/
models/
retrieval/
viz/
app/
scripts/
configs/
docs/

---

## Status

This repository contains:
- a frozen conceptual framework,
- a complete technical design,
- and an implementation plan.

Ongoing work focuses on:
- dataset preprocessing,
- encoder training,
- retrieval indexing,
- and the interactive demo.

---

## Philosophy

CLARITY is built around a simple principle:

> Learn latent structure from data, retrieve historical experience, and expose uncertainty—rather than compressing complex futures into single numbers.

---

If you’re reviewing this project and want to discuss design choices, modeling assumptions, or extensions, feel free to reach out.