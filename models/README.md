# Models

Trained weights are gitignored; only these READMEs are tracked.

- `checkpoints/` — training checkpoints. The training scripts default to
  `checkpoints/temporal_detector/<model>/<run>/` at the repo root (see
  `configs/training/temporal_detector.yaml`); copy runs here if you want to keep them.
- `production/` — models promoted for inference. Record the run, data, and metrics in
  `production/README.md` when you add one.
