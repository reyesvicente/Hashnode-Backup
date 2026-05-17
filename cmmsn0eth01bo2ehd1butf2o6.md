---
title: "Why I Built My Own Unlimited Audio & Video Transcriber"
datePublished: 2026-03-16T03:42:04.807Z
cuid: cmmsn0eth01bo2ehd1butf2o6
slug: why-i-built-my-own-unlimited-audio-video-transcriber
cover: https://cdn.hashnode.com/uploads/covers/5f95116240346172a86c20c6/3612a3ac-7567-4e18-846a-f94f21518853.png
ogImage: https://cdn.hashnode.com/uploads/og-images/5f95116240346172a86c20c6/9a26db73-eabb-40e3-a177-c69e70e49b28.png
tags: ai, productivity, python, machine-learning

---

We've all been there. You have a massive audio file—a two-hour interview, a long lecture, or a recorded meeting—and you just need it converted to text.

You do a quick search for "free audio to text transcriber" and find dozens of online tools. But there's a catch. Once you upload your file, you're hit with a paywall: *"File size exceeds limit"* or *"Upgrade to pro to transcribe more than 60 minutes."*

I recently ran into this exact problem. I needed a reliable video and audio transcriber that didn't complain when I threw a massive, multi-hour file at it. After hitting limits on almost every online service, I realized the best solution wasn't to pay for a subscription I'd rarely use—it was to build it myself.

Here is how I built my own unlimited, completely free, and private transcriber using Python, OpenAI's Whisper, and Streamlit, accelerated by NVIDIA's CUDA.

* * *

## **The Tech Stack**

I wanted the tool to be simple to use, run locally (so my data stays private), and handle both audio and video files flawlessly. Looking at my `requirements.txt`, here are the core tools making it possible:

1.  **Python:** The backbone of the project.
    
2.  **Streamlit (v1.55):** A fantastic library that turns Python scripts into interactive web apps in minutes. No HTML/CSS needed.
    
3.  **OpenAI's Whisper:** An incredibly powerful and accurate open-source speech recognition model that runs locally.
    
4.  **PyTorch & NVIDIA CUDA (cu12):** To make the transcription lightning-fast, I included `torch` along with NVIDIA dependencies like `nvidia-cudnn-cu12` and `nvidia-cublas-cu12`. This allows Whisper to harness GPU acceleration, slashing transcription times down from hours to mere minutes.
    
5.  **MoviePy (v2.2):** To handle video files seamlessly by extracting the audio tracks before passing them to Whisper.
    
6.  **python-docx & fpdf2:** To allow exporting the final transcriptions into easily shareable Word documents and PDFs.
    

## **How It Works**

The magic of the application lies in its simplicity. Here is the life cycle of a transcription task in the app:

### **1\. Handling the Uploads**

Using Streamlit's `file_uploader`, I built a quick UI that accepts various formats. Streamlit makes this incredibly easy with surprisingly few lines of code:

```python
st.title("🎙️ Audio & Video Transcriber")
st.write("Upload a file to convert speech to text.")

uploaded_file = st.file_uploader(
    "Choose a file",
    type=["mp3", "wav", "m4a", "mp4", "mov", "avi"]
)
```

### **2\. Extracting Audio from Video**

Whisper transcribes audio. But often, the content we want to transcribe is locked inside a video format (like a recorded Zoom meeting). To solve this, I integrated **MoviePy**. If the user uploads a video file, the app intercepts it, extracts the audio track in the background, and forwards *that* to the AI model:

```python
# If it's a video, extract the audio using MoviePy 2.0 syntax
if suffix.lower() in [".mp4", ".mov", ".avi"]:
    st.text("Processing video... extracting audio.")
    video = VideoFileClip(input_path)
    audio_path = input_path.replace(suffix, ".mp3")

    # Access audio and write it to a temp file
    video.audio.write_audiofile(audio_path, logger=None)
    processing_path = audio_path
    video.close() # Important to free up system resources
```

### **3\. GPU-Accelerated Transcription**

The heavy lifting is done by **Whisper**. I opted to use the `base` model, which provides a fantastic balance between transcription speed and accuracy.

Because Whisper runs locally on my machine utilizing PyTorch and my NVIDIA GPU via CUDA, **there are no time limits**. I can feed it a 3-hour podcast, and it will methodically transcribe the entire thing without ever asking me for a credit card.

To make the app snappy and avoid reloading massive AI weights on every action, I used Streamlit's `@st.cache_resource` decorator:

```python
# Initialize Whisper model, caching it to avoid reloading
@st.cache_resource
def load_model():
    # Use "base" for a good balance of speed and accuracy
    return whisper.load_model("base")

model = load_model()

def transcribe_file(file_path):
    st.info("Transcribing... This may take a few minutes.")
    result = model.transcribe(file_path)
    return result['text']
```

### **4\. Exporting the Results**

A block of text on a web page is great, but a downloadable file is better. I used `python-docx` to generate Word documents formatting the output nicely. Once the transcription is done, the app provides shiny download buttons right there in the UI:

```python
def save_as_docx(text, filename):
    doc = Document()
    doc.add_heading('Transcription', 0)
    doc.add_paragraph(text)
    doc.save(filename)
    return filename

# In the Streamlit UI later...
docx_file = "transcription.docx"
save_as_docx(transcript, docx_file)
with open(docx_file, "rb") as f:
    col1.download_button("Download as DOCX", f, file_name="transcription.docx")
```

## **The Result**

In less than 100 lines of Python code, I built a fully functional, local web app that solves a very real, very annoying problem.

**No file size limits. No hidden paywalls. Total privacy. Blazing fast GPU inference.**

Building your own tools to solve personal bottlenecks is one of the most rewarding parts of knowing how to code. If you find yourself fighting against the limitations of "free" online software, take a step back and ask yourself: *"Could I just build this?"*

Chances are, with modern libraries and a spare afternoon, you entirely can.

Code can be found here: [https://github.com/reyesvicente/audio-to-text](https://github.com/reyesvicente/audio-to-text)