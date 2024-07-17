# first-one

https://deepgram.com/learn/nova-2-speech-to-text-api

https://deepgram.com/learn/nova-2-best-speech-to-text-api-multiple-languages

https://www.gladia.io/blog/a-review-of-the-best-asr-engines-and-the-models-powering-them-in-2024

https://pytorch.org/hub/snakers4_silero-models_stt/

https://deepgram.com/learn/best-speech-to-text-apis


The latest speech-to-text models in 2024 showcase significant advancements in accuracy, speed, and multilingual support. Here are some of the top models:

Nova-2 by Deepgram: Nova-2 has been recognized for its exceptional accuracy and speed. It outperforms competitors with a median Word Error Rate (WER) of 8.4%, and is notably efficient in real-time transcription scenarios. Nova-2 also excels in handling diverse accents and noisy environments, making it a reliable choice for various applications​ (Deepgram)​​ (Deepgram)​.

Google Universal Speech Model (USM): This model is part of a family of speech models with 2 billion parameters, trained on a vast dataset covering over 300 languages. Despite its ambitious scope, practical challenges remain in achieving high accuracy across all languages. Google's ASR system is highly integrated into its ecosystem, offering robust tools for developers​ (Gladia I Audio Transcription API)​.

Microsoft Azure Speech-to-Text: Known for its customizability, Azure's model supports over 100 languages and provides features like speaker diarization and word-level timestamps. It is well-suited for enterprise applications due to its integration with the Azure ecosystem and its ability to handle domain-specific terminology​ (Gladia I Audio Transcription API)​.

Amazon Transcribe: Recently updated to support over 100 languages, Amazon Transcribe focuses on improving accuracy in underrepresented languages. It offers features like automatic punctuation and custom vocabulary, making it a versatile option for various business verticals​ (Gladia I Audio Transcription API)​.

Silero Models: These models are designed for enterprise-grade applications and support multiple languages including English, German, and Spanish. Silero models are compact and efficient, robust to various dialects and noisy environments, making them suitable for deployment in resource-constrained settings​ (PyTorch)​.

AssemblyAI: Known for its fast transcription speeds and comprehensive feature set, AssemblyAI offers decent accuracy and supports functionalities like diarization and language detection. It is a middle-of-the-road option in terms of price and performance, particularly for video and media transcription​ (Deepgram)​.

These models represent the cutting edge in speech-to-text technology, each with its strengths tailored to different use cases and environments.


import os
import openai

# Set environment variables and constants
HOME = os.environ.get("HOME")
CAFILE = HOME + "/cacert.pem"
API = "https://patagonia-aigen-srv:8080/llm-large/v1"
CHUNK_SIZE = 1024  # Define the chunk size based on the model's maximum token limit

def api_config(api):
    # Modify OpenAI's API key and API base to use vLLM's API server.
    openai.api_key = "EMPTY"
    openai.api_base = api
    os.environ["SSL_CERT_FILE"] = CAFILE

    print("[INFO] API:", openai.api_base)

    # Checking available models
    models = openai.Model.list()
    model = models['data'][0]['id']
    print(models)
    print("[INFO] Model:", model, "\n")
    return model

# Configure API and get the model
model = api_config(API)

# Function to generate a response from the model
def generate_response(history):
    response = openai.ChatCompletion.create(
        model=model,
        messages=history,
        temperature=0
    )
    return response.choices[0].message['content']

# Function to read and split the text into chunks
def read_and_split_text(file_path, chunk_size):
    with open(file_path, 'r', encoding='utf-8') as file:
        text = file.read()
    return [text[i:i+chunk_size] for i in range(0, len(text), chunk_size)]

# Function to summarize the text chunks
def summarize_text_chunks(chunks):
    summaries = []
    for chunk in chunks:
        history = [{"role": "user", "content": f"Proszę podsumuj następujący tekst:\n{chunk}"}]
        summary = generate_response(history)
        summaries.append(summary)
    return summaries

# Function to combine and refine summaries
def refine_summary(summaries):
    combined_summary = " ".join(summaries)
    history = [{"role": "user", "content": f"Proszę podsumuj następujące podsumowania:\n{combined_summary}"}]
    refined_summary = generate_response(history)
    return refined_summary

# Main function to handle the summarization process
def summarize_long_text(file_path):
    chunks = read_and_split_text(file_path, CHUNK_SIZE)
    summaries = summarize_text_chunks(chunks)
    final_summary = refine_summary(summaries)
    return final_summary

# Example usage
file_path = "path_to_your_file.txt"
final_summary = summarize_long_text(file_path)
print("Final Summary:")
print(final_summary)

