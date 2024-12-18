---
title: "How to Down-Pitch A Song Using Python"
datePublished: Wed Sep 25 2024 06:53:29 GMT+0000 (Coordinated Universal Time)
cuid: cm1hie4ia000109l6h3bfdi4f
slug: how-to-down-pitch-a-song-using-python
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1727247177460/8f3559ff-199c-462d-8275-4576f9e32423.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1727247187149/b51a00c9-2223-4fed-80d1-925a23f1cc13.jpeg
tags: music, python

---

If you’ve ever wanted to change the pitch of a song without altering its speed, this blog post is for you. Pitch-shifting is a common task for musicians, DJs, and audio engineers. In this tutorial, we will explore how to down-pitch a song using Python and the `pydub` library and apply this process to multiple songs in a folder automatically.

### Why Pitch Shift?

In music, pitch-shifting means changing the pitch of a song (raising or lowering it) without speeding it up or slowing it down. This can be useful for:

* Matching the key of a song to another track
    
* Transposing songs for instruments tuned to a different key
    
* Creating remixes or mashups
    

### Tools You Will Need

We will use the Python library `pydub` to manipulate audio files. You can install it using pip:

```bash
pip install pydub
```

Additionally, `pydub` requires `ffmpeg` to handle audio files like MP3. You can install `ffmpeg` via the terminal:

```bash
sudo apt install ffmpeg
```

### Step-by-Step Guide to Pitch Shifting

Now let’s dive into the Python script that automates pitch-shifting for multiple songs in a folder. The script loops through the files in a `songs` folder, down-pitches them by a half-step (semitone = -1), and saves the new files to an `output` folder.

#### The Code

```python
import os
from pydub import AudioSegment

# Function to shift pitch down
def pitch_shift(audio, semitones):
    # Adjust sample rate to shift pitch
    new_sample_rate = int(audio.frame_rate * (2.0 ** (semitones / 12.0)))
    return audio._spawn(audio.raw_data, overrides={'frame_rate': new_sample_rate}).set_frame_rate(audio.frame_rate)

# Input and output folders
input_folder = './songs'
output_folder = './output'

# Ensure the output folder exists
os.makedirs(output_folder, exist_ok=True)

# Loop through all files in the songs folder
for filename in os.listdir(input_folder):
    # Check if the file is an audio file (e.g., mp3 or wav)
    if filename.endswith(".mp3") or filename.endswith(".wav"):
        # Construct the full file path
        input_path = os.path.join(input_folder, filename)
        output_path = os.path.join(output_folder, filename)

        # Load the audio file
        audio = AudioSegment.from_file(input_path)

        # Shift pitch down by a half-step (semitone = -1)
        shifted_audio = pitch_shift(audio, -1)

        # Export the pitch-shifted audio to the output folder
        shifted_audio.export(output_path, format="mp3")
        print(f"Processed and saved: {output_path}")
```

#### Explanation

1. **Importing Libraries**:  
    We import `os` to work with file directories and `AudioSegment` from `pydub` to manipulate audio files.
    
2. **Pitch-Shift Function**:  
    The `pitch_shift` function adjusts the sample rate of the audio. When we change the sample rate, the pitch changes. In this case, we calculate the new sample rate to shift the pitch down by one semitone using the formula:
    
    ```python
    new_sample_rate = int(audio.frame_rate * (2.0 ** (semitones / 12.0)))
    ```
    
3. **Input and Output Folders**:  
    We define the folders where we will read the audio files and save the pitch-shifted versions. If the output folder doesn't exist, it will be created.
    
4. **Loop Through Songs**:  
    Using `os.listdir()`, we loop through each file in the `songs` folder. The script checks if the file is an audio file (`.mp3` or `.wav`) before processing it. For each file:
    
    * It loads the audio.
        
    * The `pitch_shift` function is applied, lowering the pitch by a half-step.
        
    * The pitch-shifted audio is exported to the `output` folder.
        
5. **Export and Feedback**:  
    Once the processing is done, the pitch-shifted song is saved in the `output` folder, and a confirmation message is printed.
    

### Running the Script

Make sure you have your audio files in the `songs` folder and then run the script:

```bash
python -m pitch_down
```

The pitch-shifted files will be saved in the `output` folder.

### Customization

You can easily modify this script to:

* Pitch the audio up by passing a positive value (e.g., `pitch_shift(audio, 1)` for a half-step up).
    
* Process different file formats by adding other extensions like `.ogg` or `.flac` to the conditional check.
    
* Shift by a different number of semitones by adjusting the `semitones` argument.
    

### Conclusion

This script is a simple yet powerful way to pitch-shift multiple audio files using Python. With `pydub` and `ffmpeg`, you can manipulate audio files in bulk, making tasks like pitch correction or audio preparation easier for musicians, producers, or anyone working with audio.

Feel free to experiment with this script and see how you can adapt it to your needs. Happy coding!