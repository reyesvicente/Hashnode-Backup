---
title: "Recover background audio with librosa and saving it with soundfile"
datePublished: Mon Feb 20 2023 23:00:00 GMT+0000 (Coordinated Universal Time)
cuid: cledxtx29056p5hnv4zx2frpn
slug: recover-background-audio-with-librosa-and-saving-it-with-soundfile
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1676965279223/c29ffdb0-2f44-4269-95d4-8a26ffa853ae.png

---

In my [previous article](https://dev.to/highcenburg/separate-vocals-from-a-track-using-python-4lb5) I separated the vocals from the instruments but didn't save it. This article is aimed towards saving the audio from `IPython.Audio` in `IPython`. The full spectrum, foreground spectrum and background spectrum from my [previous article](https://dev.to/highcenburg/separate-vocals-from-a-track-using-python-4lb5) looked like this: ![Audio spectrum of the track](https://cdn.hashnode.com/res/hashnode/image/upload/v1676965279223/c29ffdb0-2f44-4269-95d4-8a26ffa853ae.png)

The other goal I tied myself to was to play the audio from the instruments only which I'll also show in this article.

To save the audio, we use `soundfile`'s `write()` function and get the audio data, sample rate and save it as `.wav`

```python
import soundfile as sf

sf.write('Instruments.wav', x_background, sr, subtype='PCM_24')
y, sr = librosa.load('Instruments.wav')
ipd.Audio('Instruments.wav')
```

In recovering the background audio or instrument audio from the masked spectrum, we only use the `istft()` function from `librosa`.
```python
# Recover the background audio from the masked spectrogram
x_background = librosa.istft(S_background * phase)
# playback audio
ipd.Audio(data=x_background[90*sr:110*sr], rate=sr)
```

That's it.

To view code from the Jupyter notebook you can head over to [this article](https://colab.research.google.com/drive/1QnAhHpW9LrW9JLpjXF-xdH9Z7DtazW8Y#scrollTo=GhhM8KqaLwqV)


