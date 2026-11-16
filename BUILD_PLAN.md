# Build Plan and Time Estimate

Rough estimates for **Claude writing the code** with you reviewing and running it. They are not the same as your learning time, and they're guesses, not measurements.

## Claude's build time (active work, excluding your review and model training)

| Piece | Claude time |
|---|---|
| Repo scaffold, env, dataset download/split scripts | 15-30 min |
| CLIP-embedding + linear-probe baseline, training and eval scripts (AUC, PR, held-out generator) | 30-60 min |
| Calibration (temperature scaling, reliability diagram, ECE) | 20-30 min |
| Fine-tuned CNN/ViT variant | 30-60 min |
| C2PA metadata check | 20-30 min |
| FastAPI service + ONNX export | 45-75 min |
| React frontend (upload, verdict, confidence) | 45-90 min |
| Video: frame sampling + aggregation | 30-60 min |
| PostgreSQL logging | 20-40 min |
| **Total** | **about 4-8 hours of Claude work, spread over several sessions** |

## What is not in those numbers

- **Dataset download:** can take hours depending on size and connection.
- **Training:** depends on your GPU. The CLIP linear probe runs on a laptop CPU/GPU in minutes. Fine-tuning needs a GPU.
- **Debugging and iteration:** results will be worse than hoped on unseen generators at first. Budget extra rounds.

## Your learning time

If you build alongside Claude (recommended, since the goal is learning): about 2-4 months part-time across the four phases in the README.

## Suggested first build

Phase 1-2 only: **CLIP linear-probe baseline + evaluation on a held-out generator**. About 1-2 hours of Claude time, and it gives you a real number to improve on.
