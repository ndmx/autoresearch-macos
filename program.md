# Autonomous TinyLM Research Loop – M2 Optimized

Objective: Reach the lowest possible val_bpb on this fixed dataset & 5-minute training budget.

## Core Principles
- Be conservative with step size: prefer 1–2 focused, well-justified changes per experiment.
- If 3 consecutive experiments fail in the same direction, pivot strongly.
- Never comment out large sections or make random hyperparameter sweeps — reason first.
- Favor changes with precedent in literature or nanoGPT-style improvements (faster optimizers, better init, activation tweaks, normalization placement, etc.).
- MPS/Metal specific: avoid operations known to be very slow on Apple Silicon (certain fused ops, very large embeds, etc.). Prefer memory-efficient patterns.

## Hardware Constraint
Memory constraint: This machine has 16 GB unified memory.
Keep peak GPU memory under ~10 GB. If OOM occurs, immediately
halve batch size or model dimensions and commit as recovery.

## Required Reasoning Format (use these tags)
<thinking>
1. Current best val_bpb and git history trend (last 4–6 kept commits)?
2. Most recent failure patterns or stagnation signals?
3. One primary hypothesis for this experiment (scientific rationale).
4. Exact proposed code deltas (be surgical).
5. Expected risk/reward (low-risk incremental vs high-risk moonshot).
</thinking>

<plan>
Describe the minimal diff you will make.
</plan>

After run:
<result>
val_bpb: X.XXXX
Decision: keep / revert
Reason:
</result>

## Allowed Change Categories (in rough priority order)
1. Optimizer & scheduler (AdamW → Lion, better betas, weight decay tuning)
2. Initialization & normalization placement
3. Activation function / gating
4. Attention / feedforward dimension tweaks (within memory budget)
5. Learning rate schedule or warmup
6. Regularization (dropout location & strength, label smoothing)
7. Data / tokenization micro-improvements (if any low-hanging fruit)

Commit only if strictly better val_bpb.
Push hard but smart — aim for compound gains over 100+ cycles.
