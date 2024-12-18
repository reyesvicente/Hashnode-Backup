---
title: "Visualizing music with just a few lines of code"
datePublished: Mon Feb 06 2023 20:00:00 GMT+0000 (Coordinated Universal Time)
cuid: cleb6jjuw006moknv58uwagxz
slug: visualizing-music-with-just-a-few-lines-of-code
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1676798509578/0fe79294-c396-417d-9ee6-326f740df04d.png

---

Hey, I'm back! Let's see how far I can get with music and data science. Since I know my way around Django, I'm a little confident that I can build something with these two altogether. I'm still using librosa here, pandas, matplotlib and numpy.

You can check out the [google colab notebook](https://colab.research.google.com/drive/1Kg680zL78TAPIX1mnDfJ1vyPZeE9PU7Y?usp=sharing) if you wish to.

### Let's install and import the dependencies:
```python
pip install librosa matplotlib pandas

import matplotlib.pyplot as plt
import librosa
from librosa import display, load, amplitude_to_db, stft
import numpy as np
import pandas as pd
```

We import a couple of librosa's functions for using on our data as well as numpy, pandas and matplotlib.

### Load the data and plot the waveform
```python
y, sr = load('We Are Monsters.wav')
pd.Series(y).plot(figsize=(10, 5), lw=1, title="We Are Monsters") 
```

We load the waveform and sample rate using the `y` and `sr` variable respectively and plot the data as shown in the figure below: 

`<matplotlib.axes._subplots.AxesSubplot at 0x7f885cf29100>`

![Audio waveform of We Are Monsters by the band Dirtslub](https://cdn.hashnode.com/res/hashnode/image/upload/v1676798509578/0fe79294-c396-417d-9ee6-326f740df04d.png)

### We now create a spectrogram from our data

```python
D = stft(y)
sound_db = amplitude_to_db(np.abs(D), ref=np.max)
sound_db.shape # check the number of rows and columns in the dataset
fig, ax = plt.subplots(figsize=(10, 5))
img = display.specshow(sound_db, x_axis='time', y_axis='log', ax=ax)
```
`(1025, 11179)`

We transform the waveform(y) and get the absolute frequency of the data to produce a spectrogram and plot the data as shown in the figure below:

![Spectrogram waveform of We Are Monsters by the band Dirtslub](https://cdn.hashnode.com/res/hashnode/image/upload/v1676798511654/2cb6937e-b471-4ec1-b9cb-fe1b5107fd68.png)


### Lastly we create a melspectrogram

```python
S = librosa.feature.melspectrogram(y=y, sr=sr, n_mels=128)
s_db_mel = amplitude_to_db(S, ref=np.max)
fig, ax = plt.subplots(figsize=(10, 5))
img = display.specshow(s_db_mel, x_axis='time', y_axis='log', ax=ax)
```

Now we transform the data using the melspectrogram feature of librosa and adding 128 `n_mels` then plot the data again as shown in the image below:

![Melspectrogram waveform of We Are Monsters by the band Dirtslub](https://cdn.hashnode.com/res/hashnode/image/upload/v1676798513754/0ad51477-c7b1-4d34-98f8-4c38793de653.png)

## Conclusion
This is the continuation of my learning of python and music from day 1, which is yesterday. I may have missed some explanation on code snippets above. I would love to hear how you explain them.
