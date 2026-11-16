# AI Detection

A system that tells people whether an image or video is **real or AI-generated**, and says **how confident it is** in that answer.

> **Status:** planning / learning phase. No code yet. This README is the project plan. The original brain-dump is in [yap.txt](yap.txt).

## Why

AI-generated media is now hard to tell from real media, especially for people who didn't grow up with it. The goal is a tool that gives a clear verdict with a calibrated probability, not just a label.

## Design idea

Inspired by decision-style APIs (e.g. [Hive](https://docs.thehive.ai)), the system does **not** generate text. It takes media in and returns a **typed decision with calibrated probabilities** that software can act on:

```json
{
  "verdict": "ai_generated",
  "probability_ai": 0.93,
  "confidence_note": "calibrated (temperature scaling), val ECE 0.03",
  "provenance": { "c2pa_manifest": false }
}
```

(Example output shape only. The schema is not final.)

## Planned stack

| Layer | Tools |
|---|---|
| Language | Python |
| Image processing | OpenCV, Pillow |
| Numerics / data | NumPy, Pandas |
| Deep learning | PyTorch |
| Pretrained models | Hugging Face (CLIP, ViT) |
| Image analysis | CNN, ViT, CLIP embeddings |
| Video analysis | OpenCV + PyTorch (frame sampling) |
| API / backend | FastAPI |
| Frontend | React |
| Database | PostgreSQL |
| Deployment | ONNX Runtime |
| Provenance | C2PA tools |

## Roadmap

| Phase | Goal | Done when |
|---|---|---|
| 1. Foundations | Evaluation basics, PyTorch transfer learning | Fine-tuned a pretrained model on a small real/fake set, with a proper train/val/test split |
| 2. Detector | CLIP-embedding + linear probe baseline, then fine-tuned CNN/ViT | Reports AUC and precision/recall on a generator **not seen in training** |
| 3. Trust | Calibration, generalization tests, provenance | Reliability diagram + ECE; cross-generator eval table; C2PA check wired in |
| 4. Product | FastAPI service, ONNX export, React UI, video | Upload an image/video in the UI and get a calibrated verdict |

## Key challenges

- **Generalization:** detectors trained on one generator often fail on new ones. Evaluation always includes held-out generators.
- **Calibration:** "93% AI" must mean roughly 93% of such predictions are right (temperature scaling, reliability diagrams).
- **Data leakage:** keep train/val/test split by generator and source, not just by file.
- **Provenance:** C2PA metadata is a strong signal when present, but absence proves nothing.

## Datasets (open source)

- Hugging Face Datasets
- Kaggle (e.g. CIFAKE)
- [FaceForensics++](https://github.com/ondyari/FaceForensics)
- [ai-detected/ai-detectors](https://github.com/ai-detected/ai-detectors) (research starting point)
- GitHub-hosted benchmark sets

## Learning resources

See [RESOURCES.md](RESOURCES.md).

## Build plan and time estimate

See [BUILD_PLAN.md](BUILD_PLAN.md).

## Repo layout (planned)

```
data/        dataset download + split scripts
notebooks/   experiments
src/         model, training, eval, calibration
api/         FastAPI service
web/         React frontend
```

## License

TBD.
