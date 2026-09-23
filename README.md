# Backyard Worlds: Brown Dwarf and Mover Detection

Automated ranking of brown dwarf and moving-object candidates in Backyard Worlds WISE
infrared flipbooks, replacing manual Zooniverse classification.

Two approaches live here:

- **Unsupervised pipeline** — encode each flipbook sequence with DINOv3, cluster behaviour with
  HDBSCAN (stationary stars, fast movers, slow movers), and score candidates on motion,
  morphology, colour, novelty, and artifacts.
- **Supervised temporal detector** — 3D/2D CNNs trained on synthetic and hand-labelled ground
  truth to detect slow movers directly, with crop-level classification and heatmap inference.

**Author**: David Ackerman

Split out of the `mars_astrobio` monorepo in September 2026. Siblings: `mars_astrobio`
(CTX terrain clustering) and `mars_ctx_explorer` (CTX similarity search).

## Quick start

```bash
pixi install
pixi shell

# Unsupervised pipeline (downloads subjects from Zooniverse)
pixi run backyard-worlds

# With Zooniverse credentials for higher rate limits
backyard-worlds-pipeline --config configs/pipelines/backyard_worlds.yaml \
  --username myuser --password mypass

# Resume from an earlier run
backyard-worlds-pipeline --config configs/pipelines/backyard_worlds.yaml --skip-download --skip-encoding
```

Outputs land in `outputs/backyard_worlds/`:

- `subjects.csv` — subject metadata
- `embeddings.parquet` — 2304-dim sequence embeddings
- `subject_clusters.csv` — behaviour cluster assignments
- `brown_dwarf_ranking.csv` — ranked candidates

## Temporal detector training

```bash
# Default temporal model; also --model framestack or --model diff
pixi run train-temporal-detector --model temporal

# Crop classifier on ground-truth subjects
pixi run train-crop-classifier --help

# Full "top-random 1000" training pipeline
scripts/run_toprand_training_pipeline.sh
```

Logs and checkpoints go under `logs/temporal_detector/<model>/` and
`checkpoints/temporal_detector/<model>/`. See [docs/temporal_crop_classifier.md](docs/temporal_crop_classifier.md),
[docs/temporal_detector_experiments.md](docs/temporal_detector_experiments.md),
[docs/MOTION_FEATURES_SUMMARY.md](docs/MOTION_FEATURES_SUMMARY.md), and [docs/experiment_notes.md](docs/experiment_notes.md).

Hand-labelled ground truth (GIFs and per-subject frames) is committed under
`data/backyard_worlds/ground_truth/`.

## Layout

```
src/backyard_worlds/
├── embeddings/              # DINOv3 extractors, batched embedding pipeline
├── clustering/              # HDBSCAN clusterer, FAISS novelty detector
├── training/                # Datasets, augmentation, losses, metrics, CNN models
├── downloader.py            # Panoptes API integration
├── sequence_encoder.py, motion_sequence_encoder.py, motion_features.py
├── brown_dwarf_scorer.py, moving_object_scorer.py
├── pipeline.py              # End-to-end orchestration
└── scripts.py               # `backyard-worlds-pipeline` CLI
configs/pipelines/backyard_worlds*.yaml
configs/training/temporal_detector.yaml
scripts/                     # download, synthetic GT, training, inference, overlays, baselines
```

## Development

```bash
pixi run test
pixi run lint
pixi run format
```

## License

BSD-3-Clause. See [LICENSE](LICENSE).
