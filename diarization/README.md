# diarization

pyannote community-1 = segmentation-3.0 + ResNet34 embeddings + VBx/PLDA.
Gated on HF (access granted).

- Mirror the PLDA npz and a ResNet34 ONNX with CC BY attribution, so users
  never need gated access.
- Prove the ONNX matches the community-1 checkpoint, or adopt the weights the
  `speakrs` crate publishes.
