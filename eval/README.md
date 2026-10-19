# eval

Wrappers around the app's `lana-bench` (in the `lana` repo, `bench/`).
Numbers from a Python pipeline do not count: Lana's decode differs from the
reference pipelines at exactly the phones a model is torn on.

Baselines, 2026-10-18 (DoReCo: Sanzhi Dargwa, Svan, Nǁng, Southern English;
17 min):

| Model | Re-timing within 20 ms | Free F1 at 20 ms | Phone error rate |
| --- | --- | --- | --- |
| ZIPA small | 56% | 0.60 | 56% |
| ZIPA large | 47% | 0.51 | 51% |
| Omnilingual 300M (words) | 21% | 0.15 | – |

DoReCo's phone boundaries are WebMAUS alignments; its word boundaries were
checked by hand, so word-onset figures carry the most weight. DoReCo asks
for a citation per dataset used.
