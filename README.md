# SignSpeak — Gujarati Speech to ASL Video Converter

A pipeline that turns **spoken Gujarati audio into American Sign Language
(ASL) video**. Azure Cognitive Services handles speech recognition and
translation, Stanza does NLP lemmatization to match words to available signs,
and MoviePy stitches together the matching ASL video clips — all served
through a simple Gradio web interface.

![python](https://img.shields.io/badge/python-3.x-3776ab) ![speech](https://img.shields.io/badge/speech-Azure%20Cognitive%20Services-0078D4) ![nlp](https://img.shields.io/badge/nlp-Stanza-8A2BE2) ![video](https://img.shields.io/badge/video-MoviePy-red) ![ui](https://img.shields.io/badge/UI-Gradio-FF7C00)

---

## What it does

- **Transcribe** — converts spoken Gujarati audio into Gujarati text using
  Azure Cognitive Services Speech SDK (`gu-IN` locale).
- **Translate** — translates the Gujarati transcript into English via the
  Azure Translator API.
- **Parse & lemmatize** — runs the English text through
  [Stanza](https://stanfordnlp.github.io/stanza/) to lemmatize each word
  (so "running" and "runs" both match a single "run" sign clip), with
  `pyspellchecker` cleaning up transcription/translation typos first.
- **Generate ASL video** — looks up a pre-recorded `.mp4` clip for each
  lemmatized word in a sign-clip library and concatenates the matches with
  MoviePy into one continuous ASL video.
- **Serve via UI** — a Gradio interface lets a user upload/record an audio
  file and get back the generated sign-language video in the browser.

---

## How it works — pipeline

```
Gujarati audio (.wav)
        │
        ▼
Azure Speech-to-Text (gu-IN)  ──► Gujarati transcript
        │
        ▼
Azure Translator (gu → en)    ──► English text
        │
        ▼
Spell-check + Stanza lemmatization  ──► list of lemmatized words
        │
        ▼
Look up matching ASL clip per word (sign-clip library)
        │
        ▼
MoviePy concatenation  ──► final ASL video
        │
        ▼
Gradio interface (video output)
```

---

## Repository layout

```
├── main.py             # Full pipeline (currently a Colab notebook exported as .py):
│                        #   transcription, translation, parsing, video generation, Gradio app
├── requirements.txt     # Runtime dependencies
└── README.md
```

> **Note**: `main.py` is a Jupyter/Colab notebook saved with a `.py`
> extension (it's valid `.ipynb` JSON, not a plain script). To run it as a
> notebook, rename it to `main.ipynb` and open it in Jupyter/Colab, or
> extract the cells into a standalone script — see
> [Suggested next steps](#suggested-next-steps).

---

## Quickstart

### 1. Install dependencies

```bash
python3 -m venv venv
source venv/bin/activate

pip install -r requirements.txt
```

### 2. Set your Azure credentials (as environment variables — never hardcode these)

```bash
export AZURE_SPEECH_KEY="your-speech-key"
export AZURE_SPEECH_REGION="your-region"
export AZURE_TRANSLATOR_KEY="your-translator-key"
export AZURE_TRANSLATOR_ENDPOINT="https://api.cognitive.microsofttranslator.com/"
```

### 3. Provide a sign-clip library

The video-generation step expects one `.mp4` per lemma (e.g. `run.mp4`,
`eat.mp4`) in a local folder — point `generate_video()`'s `filePrefix` at
your own clip library.

### 4. Run it

Open `main.py` as a notebook (or run its cells as a script) — the final cell
launches the Gradio app:

```python
iface.launch(share=True, debug=True)
```

This opens a local (and optionally public, via `share=True`) URL where you
can upload a Gujarati audio clip and get back the generated ASL video.

---

## Dependencies

| Library | Role |
| --- | --- |
| `azure-cognitiveservices-speech` | Gujarati speech → text |
| `requests` | Calls the Azure Translator REST API |
| `stanza` | Tokenization, POS tagging, lemmatization |
| `pyspellchecker` | Cleans up transcription/translation typos |
| `moviepy` | Concatenates ASL clips into the final video |
| `gradio` | Web UI for upload → video output |

---

## Limitations & suggested next steps

- **Hardcoded paths**: `generate_video()` currently points at a Google Drive
  path (`/content/drive/MyDrive/Sign_language_dataset`) — this only works
  inside the original Colab environment. Parameterize this path for local/
  server use.
- **Word-for-word signing**: words are mapped to individual sign clips in
  sequence, which doesn't reflect true ASL grammar (word order, classifiers,
  non-manual markers). This is closer to Signed English / PSE than fluent
  ASL.
- **Missing clips silently skipped**: if no clip exists for a word, it's
  dropped rather than substituted or flagged — worth surfacing to the user.
- **Single source language**: currently hardcoded to Gujarati (`gu-IN` /
  `from: 'gu'`) — could be generalized to accept any Azure-supported
  source language.
- **Refactor into a script**: splitting the notebook into
  `transcribe.py`, `translate.py`, `nlp.py`, `video.py`, and `app.py` would
  make it easier to test, deploy, and reuse pieces independently.

---

## License

For educational / demonstration use.
