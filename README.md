# Lever model catalog

Public, versioned model metadata consumed by the Lever Android app. No application source, credentials, or model weights are stored here.

## Catalogs

`0_1_0.json` serves app version `0.1.0`. The filename is the app version with dots replaced by underscores.

The app requests the matching JSON file from this repository's `main` branch without authentication. It retains the last valid response and ships a bundled fallback for offline first launch. Model binaries are downloaded directly from the providers named in the catalog; their access requirements and licenses still apply.

## Chat model selection — 2026-09-22

The chat catalog contains the following five choices. Download sizes are decimal GB and come from
publisher file metadata. Minimum device RAM values are catalog eligibility thresholds, not measured
runtime peaks; the Android picker applies its existing device-memory filter.

| Model and pinned download source | Download | Minimum device RAM |
| --- | ---: | ---: |
| [Gemma-4-E4B-it (text-only)](https://huggingface.co/DarrenJiaImbue/gemma-4-E4B-it-qat-litertlm/tree/a9c6beea02917b453bd60045cb6ee23e14d91535) | 3.25 GB | 8 GB |
| [Gemma-4-E4B-it](https://huggingface.co/litert-community/gemma-4-E4B-it-litert-lm/tree/2eee7ac325f20eb8c9ac1d0e972f7c84663062da) | 3.66 GB | 12 GB |
| [Gemma-3n-E4B-it](https://huggingface.co/nmrenyi/gemma-3n-E4B-it-litert-lm/tree/a014e476a0f31ed92fc500ce6fa04591b3a8cfec) | 4.92 GB | 12 GB |
| [Gemma-3n-E2B-it](https://huggingface.co/MiCkSoftware/gemma-3n-E2B-it-litert-lm/tree/0387099e25a759df5559e3cb3052e6fc051cde73) | 3.66 GB | 8 GB |
| [MiniCPM5-2B-int4](https://huggingface.co/mlboydaisuke/MiniCPM5-2B-LiteRT/tree/013a833905ab93a33f53bf561514e18076d131e6) | 1.55 GB | 6 GB |

- Gemma 4's 2.2/2.5 GB mobile figures describe approximate inference memory, not download size.
  See [Google's memory table](https://ai.google.dev/gemma/docs/core).
- The text-only E4B is a community conversion of Google's mobile QAT checkpoint. It has a 4096-token
  context and no vision, audio or speculative-decoding sections. The official full E4B build retains
  those capabilities. The GPU/web-only artifacts are not substituted for a CPU-capable phone model.
- Gemma 3n uses Google's INT4 LiteRT-LM artifacts, not Q4 GGUF. Google’s original repositories are gated. This catalog uses public copies with SHA-256 values
  matching the originals; Lever handles Gemma terms locally without a Hugging Face account.
  No user tokens or shared developer credentials are required.
- MiniCPM uses the publisher's LiteRT INT4 conversion, not Q4_K_M GGUF. It requires LiteRT-LM 0.16+
  (the app uses 0.17.1). Its original tool-call template is preserved. No thinking toggle is advertised:
  the app's existing `enable_thinking=false` default avoids the publisher's reported INT4 reasoning
  nontermination. Model-specific tool-call quality has not been benchmarked in Lever.
- Qwen2, Qwen3 and the previous Gemma 4 E2B chat entry are removed. The separate Tiny Garden,
  Mobile Actions and Magic touch models remain for their dedicated features.

All five chat artifacts returned anonymous HTTP 206 with a LiteRT-LM header on 2026-09-22.
Schema 2 adds mandatory SHA-256 values for supported chat models. Current Lever builds check the
entire downloaded artifact before use and reject pre-migration catalogs containing gated URLs.

Gemma 3n public copies are pinned to
[MiCkSoftware E2B](https://huggingface.co/MiCkSoftware/gemma-3n-E2B-it-litert-lm/tree/0387099e25a759df5559e3cb3052e6fc051cde73)
and [nmrenyi E4B](https://huggingface.co/nmrenyi/gemma-3n-E4B-it-litert-lm/tree/a014e476a0f31ed92fc500ce6fa04591b3a8cfec).
Their file sizes and hashes match the Google originals. Lever bundles the applicable Gemma terms,
use policy and NOTICE, asks for local agreement, and copies the documents beside these model files.
Community source availability may change; replacement sources must preserve the verified hash.
