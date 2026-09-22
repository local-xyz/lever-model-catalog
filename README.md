# Lever model catalog

Public, versioned model metadata consumed by the Lever Android app. No application source, credentials, or model weights are stored here.

## Catalogs

`0_1_0.json` serves app version `0.1.0`. The filename is the app version with dots replaced by underscores.

The app requests the matching JSON file from this repository's `main` branch without authentication. It retains the last valid response and ships a bundled fallback for offline first launch. Model binaries are downloaded directly from the providers named in the catalog; their access requirements and licenses still apply.

## Chat model selection — 2026-09-22

The chat catalog contains the following five choices. Download sizes are decimal GB and come from
publisher file metadata. Minimum device RAM values are catalog eligibility thresholds, not measured
runtime peaks; the Android picker applies its existing device-memory filter.

| Model and pinned source | Download | Minimum device RAM |
| --- | ---: | ---: |
| [Gemma-4-E4B-it (text-only)](https://huggingface.co/DarrenJiaImbue/gemma-4-E4B-it-qat-litertlm/tree/a9c6beea02917b453bd60045cb6ee23e14d91535) | 3.25 GB | 8 GB |
| [Gemma-4-E4B-it](https://huggingface.co/litert-community/gemma-4-E4B-it-litert-lm/tree/2eee7ac325f20eb8c9ac1d0e972f7c84663062da) | 3.66 GB | 12 GB |
| [Gemma-3n-E4B-it](https://huggingface.co/google/gemma-3n-E4B-it-litert-lm/tree/297ed75955702dec3503e00c2c2ecbbf475300bc) | 4.92 GB | 12 GB |
| [Gemma-3n-E2B-it](https://huggingface.co/google/gemma-3n-E2B-it-litert-lm/tree/c03b6f60b8da6c5400b6838a2cf26420f80c0a01) | 3.66 GB | 8 GB |
| [MiniCPM5-2B-int4](https://huggingface.co/mlboydaisuke/MiniCPM5-2B-LiteRT/tree/013a833905ab93a33f53bf561514e18076d131e6) | 1.55 GB | 6 GB |

- Gemma 4's 2.2/2.5 GB mobile figures describe approximate inference memory, not download size.
  See [Google's memory table](https://ai.google.dev/gemma/docs/core).
- The text-only E4B is a community conversion of Google's mobile QAT checkpoint. It has a 4096-token
  context and no vision, audio or speculative-decoding sections. The official full E4B build retains
  those capabilities. The GPU/web-only artifacts are not substituted for a CPU-capable phone model.
- Gemma 3n uses Google's INT4 LiteRT-LM artifacts, not Q4 GGUF. Both repositories require the user's
  Hugging Face access and license acceptance. The install screen checks access and offers a masked
  read-token field plus links to the publisher model/license page and token settings. Only a token
  that successfully accesses the selected model is saved locally. Cancelling cannot start a download.
- MiniCPM uses the publisher's LiteRT INT4 conversion, not Q4_K_M GGUF. It requires LiteRT-LM 0.16+
  (the app uses 0.17.1). Its original tool-call template is preserved. No thinking toggle is advertised:
  the app's existing `enable_thinking=false` default avoids the publisher's reported INT4 reasoning
  nontermination. Model-specific tool-call quality has not been benchmarked in Lever.
- Qwen2, Qwen3 and the previous Gemma 4 E2B chat entry are removed. The separate Tiny Garden,
  Mobile Actions and Magic touch models remain for their dedicated features.

The pinned download URLs for both Gemma 4 variants and MiniCPM returned HTTP 200 with matching
content lengths. Gemma 3n returned the expected anonymous HTTP 401 / GatedRepo response; its pinned
file sizes were checked through the publisher metadata API. These checks establish artifact identity
and format compatibility, not a full inference benchmark on every phone.

## Updating

1. Edit the catalog for the app version you intend to support. Preserve the existing JSON schema and model identifiers.
2. Keep model artifact revisions pinned where the provider supports them. Review download URLs, file sizes, runtime capabilities and task types.
3. Validate JSON with `python3 -m json.tool 0_1_0.json > /dev/null`, then commit and push to `main`.
4. For a new app release, publish its versioned file before shipping. Copy the same reviewed catalog into the app's bundled catalog source when building that release.

Online clients pick up updates on a subsequent app launch. Invalid or unavailable responses fall back to the last valid cache, then the bundled copy. Bundled metadata does not include model weights; downloading a model still needs connectivity.

Catalog metadata is distributed under Apache-2.0; see LICENSE and NOTICE. Each model retains its publisher's license.
