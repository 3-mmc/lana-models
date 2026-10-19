# common

Helpers shared by every folder. Empty so far; things go here when a second
folder needs them, not before.

- **Kaldi fbank** matching the app's Rust implementation (ZIPA input), with a
  parity test against it.
- **Alphabets and `xsampa.tsv`**, the same table the app uses.
- **Hugging Face upload + checksum.** The app reads `x-linked-etag` from the
  302 without following it: a 64-character etag is SHA-256 (LFS), a
  40-character one is a git blob SHA-1. Check both after every upload.
- **ONNX surgery** (padding heads, moving external data, renaming outputs).
