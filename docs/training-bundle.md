# Training bundle (draft)

Status: **draft, not agreed.** The app's export is built only after this is
settled. It is the input to MMS adapter training, the ZIPA fine-tune and the
ejective probe.

A bundle is one folder (or a `.zip` of it) that a linguist exports from Lana
and keeps. Nothing uploads it: the speakers' recordings and the linguist's
corrections belong to them, and a bundle leaves the computer only when its
owner moves it.

## Layout

```
<name>.lana-bundle/
  bundle.json            what this is, who made it, licence, consent
  pieces.jsonl           one line per piece: audio file + its labels
  audio/<id>.wav         16 kHz mono 16-bit PCM, one file per piece
  source/<rec>.lana      the full annotation each piece was cut from
```

## `bundle.json`

```json
{
  "format": "lana-bundle",
  "version": 1,
  "lana": "0.1.8",
  "created": "2026-10-18T12:00:00Z",
  "language": "ka",
  "languages": ["ka", "ru"],
  "phone_alphabet": "ipa",
  "licence": "CC BY 4.0",
  "consent": { "required": true, "basis": "written", "scope": ["research", "model-training"] },
  "speakers": [{ "id": "s1", "sex": null, "age_band": null, "l1": "ka" }],
  "recordings": [{ "key": "<PROJ.key>", "name": "interview-03.wav", "duration": 1834.2, "speaker_ids": ["s1"] }]
}
```

- `language` is the main ISO 639-3 or 639-1 code; `languages` lists every code
  on the language tier.
- `phone_alphabet` is `ipa`, `arpabet` or `timit`, the phone tier's `format`.
- Speaker fields are optional except `id`.

## `pieces.jsonl`

One JSON object per line:

```json
{"id": "interview-03_0012", "audio": "audio/interview-03_0012.wav",
 "recording": "<PROJ.key>", "start": 412.36, "end": 419.02,
 "speaker": "s1", "language": "ka",
 "text": "...", "checked": true,
 "words":  [{"start": 0.00, "end": 0.41, "text": "...", "rv": true}],
 "phones": [{"start": 0.00, "end": 0.06, "text": "kʼ", "rv": true, "conf": 0.31}]}
```

- Times inside a piece are relative to its own start; `start`/`end` locate it
  in the recording.
- A piece is a stretch of one speaker in one language, at most 30 s, cut at
  word boundaries (the same 30 s pieces the built-in models already use).
- `rv` (a person decided), `conf`, `alts` and `why` are copied from the
  interval as Lana stores it. Recipes train on `rv` labels by default and use
  `conf` only to weight or filter the rest.
- `checked` is true when every word interval in the piece has `rv`.
- Empty intervals (silence) are dropped from `words`/`phones`; the gaps carry
  that information.

## Open questions

- Is consent a required field, or does the export only ask for it?
- Do we cut pieces on export, or ship whole recordings plus intervals and cut
  in the recipe? Whole recordings are smaller to describe but larger to move.
- Notes (`[tag]` marks) are left out for now. Should some tags (e.g. "bad
  audio") exclude a stretch instead?
- Does Lana later train adapters on the user's own computer, or only export
  bundles for training elsewhere? The answer decides whether the bundle needs
  to stay readable without Python.
