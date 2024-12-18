---
title: "Speech to Musical Notation with AssemblyAI"
datePublished: Wed Nov 27 2024 10:53:03 GMT+0000 (Coordinated Universal Time)
cuid: cm3zrovst001509l46rzpd7su
slug: speech-to-musical-notation-with-assemblyai
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1732704739630/4374b439-8c82-4f92-b1a4-c8c470a56c22.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1732704776788/6e12d6c2-1b60-472d-b9a0-021c20215037.jpeg
tags: ai, artificial-intelligence, music, python, machine-learning

---

*This is a submission for the* [*AssemblyAI Challenge*](https://dev.to/challenges/assemblyai) *: Sophisticated Speech-to-Text.*

## What I Built

I built Speech-to-Note, an innovative web application that combines speech recognition and musical note detection. The application allows users to record audio (either speech or singing) and processes it in two ways:

1. Converts spoken words into text using AssemblyAI's Speech-to-Text API
    
2. Analyzes the audio to detect musical notes, including their pitch, octave, and duration
    

The application features a modern, responsive UI built with React and TailwindCSS, and a robust backend powered by FastAPI. It's particularly useful for musicians, music teachers, and anyone interested in analyzing the musical properties of their voice or instruments.

## Demo

Link to site [https://speech.vicentereyes.org/](https://speech.vicentereyes.org/)

#### Landing Page

[![Landing](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F6wtue85m17rk9a96ec51.png align="left")](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F6wtue85m17rk9a96ec51.png)

#### Audio Processing

[![Processing](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fhsa4kgtyr1ihymzcjc5x.png align="left")](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fhsa4kgtyr1ihymzcjc5x.png)

[![Microphone](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Ft4mbiiuy5o09iu19x1zf.png align="left")](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Ft4mbiiuy5o09iu19x1zf.png)

[![Recording](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fygq19eib35bh536rokz2.png align="left")](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fygq19eib35bh536rokz2.png)

#### Result

[![Result](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Ffqofzg27m7m4qi1wqfxm.png align="left")](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Ffqofzg27m7m4qi1wqfxm.png)

[![Result](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fgccv22bdw67hk7xioiby.png align="left")](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fgccv22bdw67hk7xioiby.png)

## Journey

AssemblyAI's Universal-2 Speech-to-Text Model was integrated into the application through their Python SDK. The implementation can be found in the upload\_audio endpoint of our FastAPI backend:

1. When a user records audio, it's sent to our backend as a WAV file
    
2. The audio file is processed in parallel:
    
    * Sent to AssemblyAI's API for transcription
        
    * Analyzed locally using librosa for musical note detection
        
3. The transcribed text and detected musical notes are returned to the frontend
    

The AssemblyAI integration was straightforward thanks to their well-documented SDK:  

```python
transcriber = aai.Transcriber()
transcript = transcriber.transcribe(audio_file_path)
transcribed_text = transcript.text
```

What makes this implementation sophisticated is the dual-processing approach:

1. Using AssemblyAI's advanced speech recognition for accurate text transcription
    
2. Complementing it with custom pitch detection algorithms to extract musical information
    
3. Providing a synchronized playback experience where users can hear the detected notes while seeing the transcribed text
    

This creates a unique tool that bridges the gap between spoken word and musical notation, making it valuable for various musical applications, from education to composition.

The application qualifies for additional prompts as it implements:

* Real-time audio processing
    
* Custom pitch detection algorithms
    
* Interactive audio playback
    
* Modern, responsive UI with TailwindCSS
    
* Full-stack implementation with React and FastAPI
    

The project demonstrates how AssemblyAI's technology can be combined with custom audio processing to create innovative applications that go beyond simple speech-to-text conversion.