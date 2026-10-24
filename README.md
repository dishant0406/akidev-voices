# Akidev voices

A **repository** of voice models for reading aloud in Akidev. The app downloads a model from here, checks it,
and speaks with it on the device; nothing is sent anywhere while reading. A model is data (a definition that
names files to download), never code, so adding one needs no new version of the app, as long as it is a kind of model
the app already runs (`family`). Today that is `ptts`: Phonon, from Gradium.

```
repo.json              {"name": "..."}
index.json             the index (generated: see below), what the app reads first
voices/<id>.json       a model's definition (the file is named after its id)
```

## A definition

```json
{
  "id": "phonon-31m-en",
  "name": "Phonon 31M",
  "version": 1,
  "family": "ptts",
  "lang": "en",
  "languages": ["en"],
  "licence": { "name": "Apache 2.0", "url": "https://…", "commercial": true },
  "files": [
    { "path": "model.q8.gguf", "url": "https://…/resolve/<commit>/model.q8.gguf", "sha256": "…", "bytes": 64512352 }
  ]
}
```

- `family` is the runtime that speaks it. An app that does not know it shows nothing wrong, it just cannot list it.
  `min_app` (optional) is the oldest app version that can run the model.
- `lang` is the language the text is read in (how numbers and abbreviations are said); `languages` is what it can speak.
- Every file is named plainly (no folders), served over https, pinned to a commit so it cannot change under the
  definition, and checked by size and SHA-256 as it arrives. After the download the app makes the model say one sentence;
  a model that cannot is thrown away, never half-installed.
- `commercial: false` is shown to the person before they download, for licences such as CC BY-NC.

For `ptts` the files are what Phonon's runtime needs: `config.json`, `tokenizer.json`, the weights (`model.q8.gguf`)
and a voice (`default-voice.safetensors`).

## Adding a model

1. Write `voices/<id>.json`, with each file's size and hash (`shasum -a 256`).
2. Rebuild the index with the `akidev-manga` command line from the akidev repository
   (`cargo run --release -p akidev-manga --bin akidev-manga-cli -- index <this folder>`).
3. Commit both. The app lists the new model the next time Reading aloud is opened in Settings.

## What is in it

| Model | Languages | Licence | Size |
| --- | --- | --- | --- |
| `phonon-31m-en` | English | Apache 2.0 | 62 MB |

Phonon 40M is not listed: its licence (CC BY-NC 4.0) does not allow commercial use and the model is gated on Hugging Face.
