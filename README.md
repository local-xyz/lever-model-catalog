# Lever model catalog

Public, versioned model metadata consumed by the Lever Android app. No application source, credentials, or model weights are stored here.

## Catalogs

`0_1_0.json` serves app version `0.1.0`. The filename is the app version with dots replaced by underscores.

The app requests the matching JSON file from this repository's `main` branch without authentication. It retains the last valid response and ships a bundled fallback for offline first launch. Model binaries are downloaded directly from the providers named in the catalog; their access requirements and licenses still apply.

## Updating

1. Edit the catalog for the app version you intend to support. Preserve the existing JSON schema and model identifiers.
2. Keep model artifact revisions pinned where the provider supports them. Review download URLs, file sizes, runtime capabilities and task types.
3. Validate JSON with `python3 -m json.tool 0_1_0.json > /dev/null`, then commit and push to `main`.
4. For a new app release, publish its versioned file before shipping. Copy the same reviewed catalog into the app's bundled catalog source when building that release.

Online clients pick up updates on a subsequent app launch. Invalid or unavailable responses fall back to the last valid cache, then the bundled copy. Bundled metadata does not include model weights; downloading a model still needs connectivity.

Catalog metadata is distributed under Apache-2.0; see LICENSE and NOTICE. Each model retains its publisher's license.
