# apple

CoreML first; MLX only with a maintained Rust path. MLX has no maintained Rust
or C runtime (`k2-fsa/sherpa-mlx` is Python, last pushed 2025-08), so shipping
it would mean bundling Python.

1. **Measure:** `lana-bench` quick and `--eval` on an M-series Mac. No Mac
   result exists yet.
2. **whisper.cpp CoreML encoder:** `*-encoder.mlmodelc` per Whisper size,
   used through whisper-rs's `coreml` feature. The first run compiles slowly;
   the app caches it.
3. **onnxruntime CoreML provider** for ZIPA, Omnilingual, MMS, MMS-LID. Int8
   dynamic-quant graphs mostly fall back to CPU, so this needs fp16 exports, a
   second artefact per model. First check whether sherpa's static
   onnxruntime 1.28.2 for macOS includes the CoreML provider.
4. **MLX** only if 1–3 leave a large gap and mlx-c or mlx-rs proves
   maintained.
