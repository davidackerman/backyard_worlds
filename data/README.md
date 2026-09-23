# Data

- `backyard_worlds/ground_truth/` — **committed**. Hand-labelled example flipbooks (GIFs) and
  per-subject frame sets used for the temporal detector and crop classifier.
- `backyard_worlds/subjects_metadata.csv`, `subjects_metadata.csv` — **committed** subject
  metadata exports from Zooniverse.
- `backyard_worlds/subjects/` — gitignored. Downloaded flipbooks from
  `scripts/download_backyard_worlds.py` or the pipeline's download stage.
- `backyard_worlds/synthetic_*/` — gitignored. Synthetic training sets from
  `scripts/generate_synthetic_groundtruth.py`.
