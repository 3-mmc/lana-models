# lana-models

Everything that turns a model into a file [Lana](https://github.com/3-mmc/lana)
downloads: training, fine-tuning, export to ONNX, quantisation, and the Apple
runtime work. The app repo keeps only inference code, the model catalogue and
`lana-bench`.

**The contract:** each model Lana offers is a Hugging Face repo under
[`3mmc`](https://huggingface.co/3mmc) (no hyphen; GitHub is `3-mmc`) with a
model card, a licence and a `lana-bench` result. The app never builds a model;
it downloads one.

## Layout

| Folder | What | State |
| --- | --- | --- |
| `common/` | fbank, alphabets, X-SAMPA table, HF upload + checksum, ONNX surgery helpers | empty |
| `mms/` | MMS-1b-all shared part, adapters map, wide head; later adapter training | working (moved from `lana/scripts/mms`) |
| `mms-lid/` | MMS-LID re-exports | notes only |
| `zipa/` | Georgian fine-tune, ejective probe | planned |
| `ecapa-lid/` | VoxLingua107 ECAPA export | planned |
| `diarization/` | community-1 PLDA/VBx assets, ResNet34 parity | planned |
| `qwen3/` | Qwen3-ForcedAligner export | planned, low priority |
| `apple/` | CoreML encoders, fp16 exports, MLX experiments | planned |
| `eval/` | wrappers around `lana-bench --eval`, result tables | planned |
| `docs/` | the training bundle spec | draft |

A folder becomes its own repo only if it grows a separate release cycle or
other users.

## Environment

One `pyproject.toml` pins Python (3.12) and onnxruntime (1.28 series, the one
the app runs through sherpa-onnx 1.13.8). Each folder is an extra:

```sh
uv sync --extra mms          # or zipa, ecapa-lid, diarization, qwen3, apple
uv run python mms/export.py dar out/mms
```

Weights, exports and corpora stay out of git (see `.gitignore`). Local work
goes in `D:\Models`; bench results in `D:\Models\lana-eval\bench-results`.

## Rules

- **Measured by the app.** Nothing reaches the catalogue without a
  `lana-bench` row from the app's own code, against the model it would
  replace: `x_realtime` and `peak_mb` from `lana-bench --full`, CER on the
  bench clips, and for phone models `lana-bench --eval` on DoReCo. A
  fine-tune for one language is also scored on the others.
- **Loads in the app's runtime.** An export must load and match its source
  in onnxruntime 1.28 (this env), not only in the newest release.
- **Licences.** A derived model takes the most restrictive licence among its
  weights and its training data, stated on its model card. Anything trained
  on MMS stays CC BY-NC 4.0.
- **Hosting.** Hugging Face `3mmc`, or GitHub release assets only when
  upstream publishes nowhere else. The app's privacy claim names only these
  two, so a new host is a privacy change in the app.
- **Gated sources** (pyannote) are mirrored only where their licence allows
  it, with attribution.
- **Recordings stay with their owners.** A training bundle leaves a computer
  only when the user exports and uploads it.
- **Large models in WSL:** never run onnxruntime `quantize_dynamic` on a 1B
  model (8 GB RSS, it took WSL down twice). Quantise tensor by tensor under
  `ulimit -v 4000000`, as `mms/build.py` does.

## Licence

The recipes are GPL-3.0-or-later, like Lana. Each published model carries its
own licence.
