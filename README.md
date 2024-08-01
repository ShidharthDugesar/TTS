pip install SpeechRecognition pyttsx3
import tkinter as tk
from tkinter import scrolledtext
import speech_recognition as sr
import pyttsx3

# Initialize recognizer and TTS engine
recognizer = sr.Recognizer()
tts_engine = pyttsx3.init()

# Function to recognize speech and display text
def recognize_speech():
    try:
        with sr.Microphone() as source:
            status_label.config(text="Listening...")
            audio = recognizer.listen(source)
            text = recognizer.recognize_google(audio)
            text_box.insert(tk.END, text + "\n")
            status_label.config(text="Listening complete.")
    except sr.UnknownValueError:
        status_label.config(text="Could not understand the audio.")
    except sr.RequestError as e:
        status_label.config(text=f"Could not request results; {e}")

# Function to convert text to speech
def speak_text():
    text = text_input.get()
    if text:
        tts_engine.say(text)
        tts_engine.runAndWait()

# Create the main window
root = tk.Tk()
root.title("Speech to Text and Text to Speech")

# Create a scrolled text widget for displaying recognized text
text_box = scrolledtext.ScrolledText(root, width=50, height=10, wrap=tk.WORD)
text_box.grid(row=0, column=0, columnspan=2, padx=10, pady=10)

# Create a text entry widget for text-to-speech input
text_input = tk.Entry(root, width=40)
text_input.grid(row=1, column=0, padx=10, pady=5)

# Create a button to start recording and recognize speech
record_button = tk.Button(root, text="Start Recording", command=recognize_speech)
record_button.grid(row=2, column=0, padx=10, pady=5)

# Create a button to convert text to speech
speak_button = tk.Button(root, text="Speak Text", command=speak_text)
speak_button.grid(row=2, column=1, padx=10, pady=5)

# Status label
status_label = tk.Label(root, text="Status: Ready")
status_label.grid(row=3, column=0, columnspan=2, padx=10, pady=10)

# Run the main loop
root.mainloop()
