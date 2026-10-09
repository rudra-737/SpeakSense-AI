# 🎙️ SpeakSense AI

**Intelligent Speech Fluency & Communication Analyzer**

SpeakSense AI transcribes your speech with [Faster-Whisper](https://github.com/SYSTRAN/faster-whisper), measures how you speak (pace, filler words, pauses), and gives you a fluency score out of 100 with coaching feedback. It runs as a [Gradio](https://www.gradio.app/) web app inside Google Colab, so there is nothing to install locally.

## Features

- **Record or upload**: use your microphone or upload an audio file
- **Transcription**: Faster-Whisper (`tiny.en`) with word-level timestamps
- **Pace analysis**: words per minute (WPM) over the full recording
- **Filler-word detection**: counts `um`, `uh`, `erm`, `hmm`, `ah`, `like` and reports a filler rate
- **Pause estimation**: flags long gaps between words
- **Fluency score**: 0 to 100, with plain-language feedback
- **Shareable demo**: Gradio generates a temporary public link

## How it works

```
Audio input → Preprocess (16 kHz mono) → Faster-Whisper transcription
            → Feature extraction (WPM, fillers, pauses) → Scoring + feedback → Gradio UI
```

1. **Preprocessing**: audio is loaded with Librosa, resampled to 16 kHz mono, and rejected if empty or shorter than 1 second.
2. **Transcription**: Faster-Whisper `tiny.en` runs with `beam_size=5`, VAD filtering, and word timestamps. It uses GPU (`float16`) when available, otherwise CPU (`int8`).
3. **Feature extraction**: total words, WPM, filler count and rate, and pause count are computed from the transcript and word timestamps.
4. **Scoring**: a rule-based score starts at 100 and loses points for pace, fillers and pauses (see below).

## Scoring

| Factor | Rule | Max penalty |
|---|---|---|
| Pace | Target range is 90 to 170 WPM. Outside it, 15 points are deducted | 15 |
| Filler words | `filler rate (%) × 3` | 30 |
| Long pauses | Gaps of 0.75 s or more between words, normalized per minute: `pauses per minute × 2` | 20 |

If fewer than 5 words are recognized, no score is given and you are asked to record a longer sample. The thresholds are coaching heuristics, not clinical measures, and can be tuned in `evaluate_fluency()`.

## Getting started

### Run in Google Colab

1. Open `SpeakSenseAI.ipynb` in Google Colab.
2. (Optional) Switch to a GPU runtime: *Runtime → Change runtime type → T4 GPU*.
3. Run all cells in order.
4. Open the Gradio link printed by the last cell (local URL or temporary `gradio.live` link).
5. Record or upload audio and click **Analyze**.

### Dependencies

```bash
pip install faster-whisper gradio librosa soundfile pandas matplotlib jiwer
```

## Example output

The app returns three things:

- **Speech Transcript**: the recognized text
- **Speech Analytics** (JSON): fluency score, words per minute, total words, filler words, filler rate (%), long pauses, duration (seconds)
- **Personalized Feedback**: short tips on pace, fillers and pauses

## Limitations

- Pauses are **estimated from gaps between word timestamps**, not true acoustic silence detection.
- `like` is always counted as a filler, even when used normally (for example, "I like it"), so filler counts may be overestimated.
- The `tiny.en` model is fast but less accurate than larger Whisper models, and Whisper may omit or "clean up" fillers, which can cause undercounting.
- English only.
- The score is a heuristic and does not measure pronunciation, grammar, vocabulary or content quality.
- The Gradio share link is temporary (about one week) and the app stops when the Colab session ends.

## Roadmap

- Acoustic silence detection (for example with Librosa) for more accurate pauses
- Context-aware filler detection for the word "like"
- Larger Whisper models (`base.en`, `small.en`) as an option
- Pitch, volume and intonation analysis
- Progress tracking across sessions and exportable reports
- Permanent hosting on Hugging Face Spaces

## Tech stack

Python · Faster-Whisper · Librosa · SoundFile · Gradio · PyTorch · Google Colab

## License

Add your preferred license here (for example MIT).
