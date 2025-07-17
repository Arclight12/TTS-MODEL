# Date: 17-07-2025

## ✅ Today's Work

- Successfully ran the **FastSpeech2 model locally**.  
- Installed all required dependencies, including the **SpeechBrain library** for running the model.  
- The model was **able to convert phonemes into intelligible audio output**.  
- Explored the **role of an aligner** required during the **training process** of the model.  

---

## 📌 Findings on Aligner  

1. **Dataset Coverage**  
   - The FastSpeech2 model (trained on **LJSpeech**) already covers most of the English dictionary (excluding scientific or domain-specific terms).  
   - **Retraining is only needed** when you want to:  
     - Change the **voice style**  
     - Change or add a **new language/accent**  

2. **When Do You Need an Aligner?**  
   - An aligner like **Montreal Forced Aligner (MFA)** is required to **match phoneme frames with audio frames**.  
   - **Inference does NOT need an aligner**, because the model is already pre-trained to map phoneme → audio.  
   - Aligners are **only needed for training** to generate correct phoneme-to-audio duration mappings.  

3. **Aligner Workflow for Training**  
   - **Collect Dataset**  
   - **Preprocess Data**  
     - Convert text → phonemes  
     - Convert audio → mel spectrogram  
   - **Run Aligner (e.g., MFA)** to map phoneme frames ↔ audio frames  
   - **Train the model** with aligned data  

---

## 📝 Notes  

### 🎼 Montreal Forced Aligner (MFA) & Alternatives  

- **MFA** → A tool that aligns text (converted to phonemes) with the audio timeline. It outputs phoneme **durations**.  
- **Alternatives:**  
  - **Gentle** → Lightweight forced aligner for English  
  - **ProsodyLab Aligner** → Another forced aligner based on Kaldi  
  - **Aeneas** → Mostly used for audiobook/text alignment  
  - Any other **Kaldi-based forced aligner**  

👉 **In simple terms:**  
An aligner tells the model **which part of the audio corresponds to which phoneme**, so it can learn correct **durations**.  

---

### 🏗️ FastSpeech2 Training Pipeline (Step-by-Step)  

1️⃣ **Collect Dataset**  
- Record or download audio clips + transcripts  
- Examples: **LJSpeech, VCTK, or your own recordings**  

2️⃣ **Preprocess Text & Audio**  
- Convert transcripts → **phonemes**  
- Convert audio → **mel spectrograms**  

3️⃣ **Run Aligner (MFA)**  
- Align phonemes with audio → get **phoneme durations**  

4️⃣ **Prepare Training Data**  
- Combine **phoneme sequence + phoneme durations + mel spectrogram**  

5️⃣ **Train FastSpeech2**  
- Model learns: **Phonemes + Durations → Mel spectrogram**  

6️⃣ **Train/Reuse Vocoder**  
- Use **HiFiGAN** to convert mel spectrogram → final audio  

7️⃣ **Inference**  
- **Phoneme Input → FastSpeech2 → Mel Spectrogram → HiFiGAN → Audio Output**  

---


### 🧑‍💻 Example: Inference Code used for running locally.

```python

import warnings
warnings.filterwarnings("ignore")

import os
os.environ["HF_HUB_DISABLE_SYMLINKS_WARNING"] = "1"

import torch
import soundfile as sf
from speechbrain.inference.TTS import FastSpeech2
from speechbrain.inference.vocoders import HIFIGAN

# Load FastSpeech2 from your local folder
fs = FastSpeech2.from_hparams(
    source="C:/Users/yedhu/Documents/TTS_Models/fastspeech2_ljspeech",
    savedir="C:/Users/yedhu/Documents/TTS_Models/fastspeech2_ljspeech"
)

# Load HiFiGAN vocoder from your local folder
hi = HIFIGAN.from_hparams(
    source="C:/Users/yedhu/Documents/TTS_Models/hifigan_ljspeech",
    savedir="C:/Users/yedhu/Documents/TTS_Models/hifigan_ljspeech"
)

# Define the phoneme sequence for "This is a test"
phonemes = ['DH', 'AH', 'S', 'IH', 'S', 'T', 'EH', 'S', 'T']

# Generate mel spectrogram
mel, *_ = fs.encode_phoneme([phonemes])

# Convert mel spectrogram to audio waveform
wave = hi.decode_batch(mel)

# Save as WAV file
sf.write("output.wav", wave.squeeze().cpu().numpy(), 22050)
print("✅ Audio saved as output.wav")

# (Optional) Auto-play on Windows
import os
os.system("start output.wav") 
```


