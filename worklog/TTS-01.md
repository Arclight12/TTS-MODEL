# Date: 14-07-2025

## ✅ Today's Work

- Tested two pre-trained models (**Tacotron** and **Glow-TTS**) from the **Coqui-TTS** library in Google Colab.  
- Both models produced **proper audio output** when given **text input**.  
- When given **phoneme input**, the models **failed to produce correct audio**.

---

## 📌 Findings

### 1. Model Training Data  
- All the models are trained on **different datasets** for each library.  
- **Coqui-TTS models** are trained with **text input**, **not phonemes**.  
- As a result, they **do not handle phoneme input well**.  
- When phonemes are provided, the **output audio is flawed** (e.g., words are spelled out separately).

---

### 2. Internal G2P Conversion  
- These models perform **internal Grapheme-to-Phoneme (G2P)** conversion.  
- Supplying phonemes directly **interferes with this process**, leading to **incorrect or unnatural audio output**.

---

### 3. Phoneme Formats  
- There are two main phoneme formats:  
  - **IPA** (International Phonetic Alphabet)  
  - **ARPAbet**  
- **Coqui-TTS models only accept ARPAbet** phonemes.

---

## 📝 Notes

- Even though **audio is generated from phoneme input**, it is **not natural or correct** due to the reasons above.  
- Future work should consider the **input requirements** and **internal processing** of the models being used.
- If we want to work with these two model, specifically glow-tts. We will need to train the model on phoneme as input dataset with respective audio files.

---

## ▶️ Example Script to Run Tacotron Model

```python
from TTS.api import TTS
from IPython.display import Audio

# Initialize TTS with the desired model
# Use the correct model name from the list of available models
tts = TTS("tts_models/en/ljspeech/tacotron2-DDC")

# Define the text you want to convert to speech
input_text = "Hello, this is a test of the Tacotron model."

# Synthesize the speech
# The output file will be saved as 'output.wav' in the current directory
tts.tts_to_file(text=input_text, file_path="output.wav")

# Play the audio
audio = Audio("output.wav")
display(audio)
```

## ▶️ Example Script to Run Glow-TTS Model

```python
from TTS.api import TTS
from IPython.display import Audio

# Initialize TTS with the Glow-TTS model
tts = TTS("tts_models/en/ljspeech/glow-tts")

# Define the text you want to convert to speech
input_text = "Hello, this is a test of the Glow-TTS model."

# Synthesize the speech
# The output file will be saved as 'glow_output.wav' in the current directory
tts.tts_to_file(text=input_text, file_path="glow_output.wav")

# Play the audio
audio = Audio("glow_output.wav")
display(audio)
```


