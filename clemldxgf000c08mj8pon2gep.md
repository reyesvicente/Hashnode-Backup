---
title: "Suppressing audio with Python"
datePublished: Mon Feb 27 2023 09:02:56 GMT+0000 (Coordinated Universal Time)
cuid: clemldxgf000c08mj8pon2gep
slug: suppressing-audio-with-python
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1682766435447/07c5cacf-66e0-44c2-8a61-e41e0dc6f0f2.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1677488567095/c7c6b254-589a-4463-99f7-a3d6abf78e27.avif
tags: music, python

---

In my [previous article](https://vicentereyes.org/separate-vocals-from-a-track-using-python), I separated the vocals from a track using `librosa`. I wasn't happy about the outcome so I did a little googling and found another audio library from python called [noisereduce](https://pypi.org/project/noisereduce/). In this article, I'll show you how I solved my problem with a muddy audio which was removed using `librosa`.

You can find the [jupyter notebook here](https://colab.research.google.com/drive/1QnAhHpW9LrW9JLpjXF-xdH9Z7DtazW8Y?usp=sharing).

```python
# Read audio
data, samplerate = sf.read('Vocals.wav')
# reduce noise
y_reduced_noise = nr.reduce_noise(y=data, sr=samplerate)
# save audio
sf.write('Vocals_reduced.wav', y_reduced_noise, samplerate, subtype="PCM_24")
# load and play audio
data, samplerate = librosa.load('Vocals_reduced.wav')
ipd.Audio('Vocals_reduced.wav')
```

We first read the audio's `y` and `x` axis with a data and samplerate variable with `soundfile`. Then reduce the noise with the `reduce_noise()` function of `noisereduce` which we then pass in the `data` and `samplerate` arguements for the function. Next, we write the new audio with `soundfile`'s `write()` function and pass in the reduced noise variable, samplerate to get a `.wav` output. Finally, we load and play the audio with `librosa`'s `load()` function and `IPython`'s `Audio()` function.