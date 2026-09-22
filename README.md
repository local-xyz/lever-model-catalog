# Lever model catalog

Public, versioned model metadata consumed by the Lever Android app. No application source, credentials, or model weights are stored here.

## Catalogs

`0_1_0.json` serves app version `0.1.0`. The filename is the app version with dots replaced by underscores.

The app requests the matching JSON file from this repository's `main` branch without authentication. It retains the last valid response and ships a bundled fallback for offline first launch. Model binaries are downloaded directly from the providers named in the catalog; their access requirements and licenses still apply.

## Agent Chat models — 2026-09-22

Agent Chat matches [Google Edge Gallery's catalog](https://github.com/google-ai-edge/gallery/blob/6353707057ccc524a6e513e73f0d6d5886f348a8/model_allowlists/1_0_19.json).
Only these two models are supported:

| Model | Download | Minimum device RAM |
| --- | ---: | ---: |
| Gemma-4-E2B-it | 2.59 GB | 8 GB |
| Gemma-4-E4B-it | 3.66 GB | 12 GB |

Model repositories, filenames, revisions, capabilities, task associations and default inference
settings match that Gallery snapshot (also checked against upstream on 2026-09-22). Both default
to GPU, with CPU available. Thinking and speculative decoding remain excluded from Agent Chat,
as in Gallery. SHA-256 values additionally pin the downloaded bytes for Lever's integrity check.
Both pinned sources returned anonymous HTTP 206 with LiteRT-LM headers; no access token is needed.

The community text-only E4B, Gemma 3n E2B/E4B and MiniCPM entries are removed. Tiny Garden,
Mobile Actions and Magic touch remain exclusive to their separate features. The picker retains
its device-memory filter, making E2B the suggested choice on an 8 GB Pixel.

Catalog schema 3 prevents old remote or cached catalogs from restoring removed Agent Chat models.
An updated APK falls back to its bundled schema 3 catalog when the network/cache still has schema 2.
Previously downloaded files and chat history are preserved; removed models are no longer selectable.
Pending setup for an unavailable artifact is discarded, and cancellation cannot restore an
unavailable original model. Install a supported model to continue if the old selection was removed.
Older APKs that require schema 2 need the app update to receive this catalog.
