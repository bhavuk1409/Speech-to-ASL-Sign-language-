# Speech-to-ASL-Sign-language-
# Speech-to-ASL Sign Language Converter

This project converts spoken Gujarati audio into American Sign Language (ASL) video clips. It uses Azure Cognitive Services for speech recognition and translation, natural language processing with Stanza, and video processing with MoviePy.

## Features

- **Speech Recognition**: Transcribes Gujarati audio to text using Azure Cognitive Services.
- **Translation**: Translates Gujarati text to English using Azure Translator.
- **Text Processing**: Corrects spelling errors and parses text using Stanza and SpellChecker.
- **Sign Language Video Generation**: Generates an ASL video by concatenating video clips corresponding to the translated and parsed text.
- **User Interface**: A Gradio-based interface that allows users to upload audio files and receive sign language videos.

## How It Works

1. **Transcribe Audio**: Converts audio input into text using Azure Cognitive Services.
2. **Translate Text**: Translates the transcribed text from Gujarati to English.
3. **Text Parsing**: Processes and parses the translated text to identify key words or phrases.
4. **Generate Video**: Matches parsed words with corresponding ASL video clips and concatenates them to form a complete sign language video.
5. **Display Video**: The final ASL video is displayed in the Gradio interface.

## Requirements

Ensure you have the following dependencies installed:

```plaintext
gradio
azure-cognitiveservices-speech
moviepy
pyspellchecker
stanza
requests
