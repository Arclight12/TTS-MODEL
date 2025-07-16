# Date: 16-07-2025

## ✅ Today's Work

- Today, we tested and ran models from a few **different libraries** to check if any of them can **convert phonemes to audio correctly**.  
- The models tested were **VITS** and **FastPitch**.  
- We ran the **FastPitch model** from the **NeMo library (NVIDIA)**.  
- For **VITS**, we tried multiple implementations from **ESPnet** and **Coqui TTS**.  
- All these models were able to produce audio, but **none were intelligible or proper when given phoneme input**.  

---

## 📌 Findings  

1. So far, we have checked **three libraries** and **multiple models**, and all have the **same issue**:  
   - None were able to produce **proper audio** when **phonemes** were given as input.  

2. The main problem seems to be that **most pre-trained models are not trained on phonemes as input**.  
   - While they *accept* phonemes as input, they **can’t produce proper output** because they learned **text-to-audio mappings**, not **phoneme-to-audio mappings**.  

3. We also tested **FastPitch** (a more advanced version of FastSpeech2) from NVIDIA’s NeMo library.  
   - However, even this model was **not able to produce correct audio** with phoneme input.  

4. According to online sources, **ESPnet** previously had models trained with **phoneme input**,  
   - But many of these models have either been **discontinued** or **restricted** for access.  

---

## 📝 Notes  

### 🎼 FastPitch vs FastSpeech2 – Easy Difference  

- **FastSpeech2**  
  - A **non-autoregressive** TTS model.  
  - Predicts **duration, pitch, and energy** before generating the mel-spectrogram.  
  - Needs an **aligner** during training.  

- **FastPitch**  
  - Based on **FastSpeech2**, but adds **better pitch prediction** for improved tone and naturalness.  
  - Also faster and more controllable.  

✅ **Key takeaway:** FastPitch = FastSpeech2 **+ better pitch handling**.  

---

### 🏗️ Architecture of Different Models (Simple Overview)  

Here’s how the main TTS models are built, in **simple terms**:  

---

#### 1️⃣ Tacotron2  
- Uses a **Seq2Seq model with attention** (like an RNN-based translator).  
- Generates mel-spectrogram **step by step** → needs a **vocoder** (like HiFi-GAN) to produce audio.  
✅ Natural sounding but **slow** because it’s autoregressive.  

---

#### 2️⃣ Glow-TTS  
- Uses a **flow-based model** to map text directly to mel-spectrograms.  
- Predicts **duration explicitly**, so no attention needed.  
✅ Faster and more stable than Tacotron2, but still needs a **vocoder**.  

---

#### 3️⃣ VITS  
- Combines several techniques (**VAE + Flow + GAN**) in **one end-to-end model**.  
- Directly generates **waveforms** from text/phonemes → **no separate vocoder needed**.  
✅ High quality, end-to-end. ❌ But more complex to train.  

---

#### 4️⃣ FastSpeech2  
- Built on a **Transformer** (like in NLP).  
- Predicts **duration, pitch, and energy**, then generates mel-spectrogram **in parallel** (not step by step).  
- Needs a **vocoder** for final audio.  
✅ Much faster & stable compared to Tacotron2.  

---

#### 5️⃣ FastPitch  
- Same **Transformer backbone** as FastSpeech2.  
- Adds **pitch-aware prediction** for better intonation control.  
✅ Faster + better pitch control.  

---

✅ **Quick Summary Table**  

| Model        | Autoregressive? | Core Idea             | Needs Vocoder? |
|--------------|-----------------|-----------------------|---------------|
| **Tacotron2** | ✅ Yes          | Seq2Seq + Attention   | ✅ Yes |
| **Glow-TTS**  | ❌ No           | Flow-based model      | ✅ Yes |
| **VITS**      | ❌ No           | VAE + Flow + GAN      | ❌ No |
| **FastSpeech2** | ❌ No         | Transformer-based     | ✅ Yes |
| **FastPitch**  | ❌ No          | Transformer + Pitch   | ✅ Yes |

---

✅ **In simple words:**  
- **Tacotron2** → Old-style **Seq2Seq**, slow but natural.  
- **FastSpeech2 & FastPitch** → **Transformer-based**, very fast.  
- **Glow-TTS** → Uses **flow models**, also fast.  
- **VITS** → Fully end-to-end, no vocoder needed.  

---

