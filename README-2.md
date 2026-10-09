# 🎙️ SpeakSense AI

### Intelligent Speech Fluency & Communication Analyzer

SpeakSense AI is a Python-based speech processing capstone project that converts spoken English into text and analyzes measurable speech patterns. Using a pretrained **Whisper** speech recognition model, it estimates speaking speed, identifies selected filler words, detects long gaps between recognized words, and offers personalized feedback through an interactive **Gradio** interface.

> **Note:** SpeakSense AI is a communication-practice tool, not a diagnostic assessment. Its fluency score is based on configurable rules and has not been clinically validated.

## ✨ Features

- 🎤 **Record speech** directly in the browser or upload an audio file.
- 📝 **Automatic transcription** using Whisper (`base.en`).
- ⏱️ **Speaking pace** in words per minute (WPM).
- 🔎 **Filler-word detection** for words such as *um*, *uh*, *erm*, *hmm*, and *ah* when transcribed.
- ⏸️ **Long-gap estimation** from recognized word timestamps.
- 📊 **Fluency score** (0–100) calculated using explainable, rule-based heuristics.
- 💡 **Actionable feedback** on pacing, filler words, and pauses.
- 🌐 **Gradio web UI** with separate recording and uploading tabs.

## 🧠 How It Works

```text
Microphone / Audio Upload
            ↓
Audio Preprocessing (16 kHz, mono)
            ↓
Whisper Speech-to-Text + Word Timestamps
            ↓
Feature Extraction
  ├─ Speaking speed (WPM)
  ├─ Filler-word frequency
  └─ Long gaps between recognized words
            ↓
Rule-Based Scoring & Feedback
            ↓
Gradio Results Dashboard
```

## 🛠️ Tech Stack

| Component | Technology |
|---|---|
| Language | Python |
| Development environment | Google Colab |
| Speech recognition | faster-whisper (`base.en`) |
| Audio preprocessing | librosa, NumPy, SoundFile |
| Speech feature analysis | Python, regular expressions |
| Interface | Gradio |
| Optional evaluation | jiwer (word error rate), pandas |
| Optional visualization | Matplotlib |

## 🚀 Run in Google Colab

1. Open [Google Colab](https://colab.research.google.com/).
2. Upload and open your project notebook (for example, `SpeakSense_AI.ipynb`).
3. Optionally select **Runtime → Change runtime type → T4 GPU** if available. CPU works too, but transcription can be slower.
4. Run the notebook cells from top to bottom. The dependencies installation cell is:

   ```python
   !pip install -q faster-whisper gradio librosa soundfile pandas matplotlib jiwer
   ```

5. Wait for the pretrained model to load; its weights will be downloaded the first time.
6. Launch the Gradio application from the final cell:

   ```python
   demo.launch(share=True, debug=True, inline=False)
   ```

7. Open the generated HTTPS Gradio link, allow microphone permission in your browser, and choose **Record Voice** or **Upload Audio**.
8. Click the corresponding **Analyze** button to view your transcript, speech metrics, and recommendations.

> The public Gradio share link is temporary. It is intended for demos, not permanent production hosting. Do not share recordings containing sensitive information.

## 💻 Local Setup (Optional)

Prerequisites: Python 3.10+ and a browser with microphone permissions.

```bash
python -m venv .venv
source .venv/bin/activate   # On Windows: .venv\Scripts\activate
pip install faster-whisper gradio librosa soundfile pandas matplotlib jiwer
```

To run the project locally, copy the notebook's imports, model loading, analysis functions, and Gradio interface into a Python script such as `app.py`, then execute:

```bash
python app.py
```

*Note: An `app.py` file is not included automatically when using the Colab notebook; it must be created separately for this local workflow.*

## 📊 Speech Metrics

| Metric | Description |
|---|---|
| Words per minute (WPM) | Transcribed word count divided by total recording duration, multiplied by 60 |
| Filler count | Number of selected filler words found in recognized word timestamps |
| Filler rate | Filler count divided by recognized word count, expressed as a percentage |
| Long pauses | Estimated gaps of at least 0.75 seconds between consecutive recognized words |
| Fluency score | A heuristic 0–100 score based on speaking pace, filler rate, and long-gap frequency |

The baseline target speaking pace is **90–170 WPM**. These thresholds and penalties are coaching heuristics, not universal standards. Word-level timestamps and filler recognition may be imperfect; in particular, long word gaps are **not** the same as verified acoustic silence.

## 🧪 Testing & Evaluation

Recommended test cases include clear speech, intentionally slow or fast speech, samples with filler words, samples with long breaks, and recordings with background noise.

For a stronger capstone evaluation, collect short recordings from consenting participants and annotate reference transcripts, fillers, and pauses. Useful evaluation measures include:

- **Word Error Rate (WER):** Compare model transcripts with manually prepared references using `jiwer`.
- **Filler detection precision and recall:** Compare detected fillers with human annotations.
- **Pause-count error:** Compare predicted long-gap counts with annotated pauses, while noting timestamp limitations.
- **Latency:** Measure processing time per recording.

No model-accuracy results are claimed here; report measured results only after conducting experiments.

## ⚠️ Limitations

- The initial model is designed for **English** speech.
- Background noise, accents, and poor recording quality may affect transcription.
- Whisper may omit hesitation sounds, causing filler words to be undercounted.
- Gaps between word timestamps are only an approximation of pauses.
- The fluency score is rule-based; it should not be interpreted as a validated measure of speaking ability.
- The system does not diagnose stuttering or any other medical condition.

## 🔮 Future Enhancements

- Before-and-after practice session comparisons.
- Historical progress tracking and charts.
- More robust acoustic voice-activity detection for silence analysis.
- Pronunciation-focused feedback with suitable evaluation methods.
- Multilingual speech support.
- Exportable progress reports.

## 🎓 Academic Use

**Project:** SpeakSense AI – Intelligent Speech Fluency & Communication Analyzer  
**Domain:** Artificial Intelligence · Speech Processing · Natural Language Processing  
**Project type:** B.Tech AI & ML Capstone

## 📚 References

- [faster-whisper](https://github.com/SYSTRAN/faster-whisper)
- [OpenAI Whisper](https://github.com/openai/whisper)
- [Gradio Documentation](https://www.gradio.app/docs)
- [Librosa Documentation](https://librosa.org/doc/latest/index.html)
- [JiWER](https://github.com/jitsi/jiwer)

---

*Built for learning and experimentation in AI-powered speech processing.*
