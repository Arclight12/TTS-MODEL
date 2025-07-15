Date- 14-07-2025



Todays work:



I tried to run two pre trained model from coqui-TTS library/Framework in google colab. when the input was given as text, both the model were able to output proper audio.However, when i gave phoneme as an input, it wasnt able to give proper output/audio.



The reason being so:-



1\. All the model are trained in different datasets for each library. So, for coqui-TTS library the tacotron and glow-TTS model as been trained with text as an input not phoneme. That's why when we give input as a text, it isnt able to properly recognise it and produce proper output.



We are getting an output/audio. However, there are major flaws in the audio so produced.

for example, when we gave input= how are you(in phoneme format).

the output audio so produced spells each word seperatly.



2\. The models are such that they internally undergo text to phoneme conversion. The grapheme to phoneme conversion(G2P). Therefore, when we give phoneme as an input it collides with the G2P and the natural process. This causes the whole conversion process to become more complex. Therefore the output so produced is not correct.



Few other things to note are:-



The phoneme format are of two type, they are IPA(International phonetic Association) and ARPAbet.The models from coqui-TTS only take the phoneme in ARPABET format.



