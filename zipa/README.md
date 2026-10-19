# zipa

Lana downloads `anyspeech/zipa-*-crctc-500k` (CC BY 4.0) as published.

## Georgian fine-tune (workstream 3)

- **Data:** Common Voice ka (CC0) + FLEURS ka; later Lana training bundles.
- **Labels:** grapheme-to-IPA that keeps ejectives (კ პ ტ წ ჭ ყ).
- **Output:** `3mmc/zipa-small-ka`, ONNX, same 128-token vocabulary.
- **Gate:** no loss on the other DoReCo languages.

## Ejective probe (workstream 4)

- **Input:** ZIPA log-probs (128 per 20 ms frame, ±k frames).
- **Labels:** Georgian weak labels.
- **Output:** a few-KB linear/MLP head, as JSON or ONNX.
- **Validate on:** DoReCo Svan, Sanzhi, Tabasaran (CC BY). The app's
  zero-shot cue track comes first.
