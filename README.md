# Arabic Audio Understanding and Retrieval System

## Overview

This project is an Arabic Audio Understanding and Retrieval System that allows users to upload an Arabic audio file, automatically transcribe it, summarize its content, search inside the transcript semantically, and ask questions about the audio.

The system combines multiple NLP and speech processing tasks into one complete pipeline:

Arabic Audio → Whisper ASR → Arabic Transcript → mT5 Summarization → Transcript Chunking → E5 Semantic Search → Grounded Extractive Answer

The final system is deployed using Gradio, where users can upload audio, view the transcription and summary, and ask Arabic questions about the audio content.

---

## Features

- Arabic speech-to-text transcription using Whisper.
- Arabic text summarization using mT5.
- Arabic semantic search using multilingual E5 embeddings.
- Retrieval-Augmented Question Answering over audio transcripts.
- Relevance checking to avoid answering unrelated questions.
- Gradio interface for easy interaction.
- Evaluation metrics for ASR, summarization, retrieval, and question answering.

---

## Project Tasks

### Task 1: Arabic Automatic Speech Recognition

The system uses openai/whisper-medium to convert Arabic audio into written Arabic text.

Evaluation metrics:

- WER: Word Error Rate
- CER: Character Error Rate

These metrics are used to measure transcription quality.

---

### Task 2: Arabic Text Summarization

The project uses csebuetnlp/mT5_multilingual_XLSum to summarize Arabic transcripts.

Evaluation metrics:

- ROUGE-1
- ROUGE-2
- ROUGE-L

These metrics compare the generated summary with a reference summary.

---

### Task 3: Arabic Semantic Search

The system first tests multilingual MiniLM, then uses E5 in the final Gradio demo.

Final semantic search model:

- intfloat/multilingual-e5-small

E5 uses prefixes:

- query: user question
- passage: transcript chunk

Evaluation metrics:

- Precision@K
- Recall@K
- Hit@K
- MRR

These metrics evaluate how well the system retrieves relevant text chunks.

---

### Task 4: Retrieval-Augmented Question Answering

The system retrieves the most relevant transcript chunks and generates a grounded answer based on them.

The answer generation is extractive and grounded, meaning the system answers only from the retrieved transcript content.

Evaluation metrics:

- Exact Match
- Token F1
- Context Support Rate
- Gold Answer in Top Context Rate

These metrics evaluate answer correctness and whether the answer is supported by the retrieved context.

---

## Final Gradio Demo

The final demo allows the user to:

1. Upload an Arabic audio file.
2. Generate the transcript using Whisper.
3. Generate a summary using mT5.
4. Split the transcript into searchable chunks.
5. Ask Arabic questions about the audio.
6. Retrieve the most relevant transcript chunks using E5.
7. Generate a grounded answer.

The Gradio interface has two main tabs:

### 1. Process Audio

This tab outputs:

- Processing status
- Full transcript
- Summary
- Transcript chunks

### 2. Ask Questions

This tab outputs:

- Grounded answer
- Retrieved audio segments
- Retrieved context

---

## Relevance Filtering

Semantic search always returns the closest result, even if the question is unrelated to the audio.

To avoid incorrect answers, the project includes a relevance-checking step based on:

- Semantic similarity score
- Keyword overlap
- Difference between top retrieved results

If the retrieved content is not relevant enough, the system returns:

لا أعلم، لم أجد في التسجيل جزءًا واضحًا يتحدث عن هذا الموضوع.

This helps reduce hallucination and prevents the system from forcing unrelated answers.

---

## Models Used

| Component | Model |
|---|---|
| ASR | openai/whisper-medium |
| Summarization | csebuetnlp/mT5_multilingual_XLSum |
| Initial Semantic Search | sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2 |
| Final Semantic Search | intfloat/multilingual-e5-small |
| Interface | Gradio |

---

## Evaluation Metrics

| Task | Metrics |
|---|---|
| ASR | WER, CER |
| Summarization | ROUGE-1, ROUGE-2, ROUGE-L |
| Semantic Search | Precision@K, Recall@K, Hit@K, MRR |
| RAG Question Answering | Exact Match, Token F1, Context Support Rate, Gold Answer in Top Context Rate |

---

## Installation

Install the required libraries:

- transformers
- sentence-transformers
- datasets
- evaluate
- jiwer
- rouge-score
- gradio
- torch
- pandas
- numpy
- scipy

You also need FFmpeg for audio processing.

On Windows, you can install FFmpeg using:

winget install Gyan.FFmpeg

After installation, check that FFmpeg works:

ffmpeg -version

---

## How to Run

1. Clone the repository:

git clone https://github.com/your-username/arabic-audio-rag-system.git

cd arabic-audio-rag-system

2. Install the requirements:

pip install -r requirements.txt

3. Open the notebook:

jupyter notebook

or open it in VS Code.

4. Run the notebook cells in order.

5. Launch the Gradio demo.

6. Upload an Arabic audio file and ask questions about it.

---

## Example Workflow

User uploads Arabic audio  
↓  
Whisper transcribes the audio  
↓  
mT5 summarizes the transcript  
↓  
Transcript is split into chunks  
↓  
E5 embeds the chunks  
↓  
User asks a question  
↓  
E5 retrieves relevant chunks  
↓  
System generates grounded answer

---

## Example Questions

After uploading Arabic audio, users can ask questions such as:

- ما الموضوع الرئيسي في التسجيل؟
- هل هناك شيء يخص الرياضة؟
- ما الفائدة المذكورة في التسجيل؟
- هل يتحدث التسجيل عن التعليم؟

If the topic is not found in the audio, the system should say that it does not know.

---

## Limitations

- ASR errors can affect summarization and retrieval quality.
- The summarization model may perform better on formal Arabic than casual spoken Arabic.
- Timestamps are approximate.
- The final answer is extractive, not generated by a full large language model.
- Relevance thresholds may need tuning for different audio types.

---

## Future Work

Possible improvements include:

- Fine-tuning Whisper on more Arabic speech data.
- Adding punctuation restoration to Arabic transcripts.
- Using a stronger Arabic summarization model.
- Adding speaker diarization.
- Using a full Arabic LLM for answer generation with retrieved context.
- Improving timestamp accuracy.
- Supporting multiple audio files.
- Storing transcript embeddings in FAISS or another vector database.

---

## Project Structure

arabic-audio-rag-system/

- Project-2-NLP.ipynb
- README.md
- requirements.txt
- reports/
  - Arabic_Audio_RAG_Project_Report.pdf
  - Arabic_Audio_RAG_Evaluation_Metrics_Cheat_Sheet.pdf
- data/
  - audio/

---

## Requirements File

You can create a requirements.txt file with:

transformers  
sentence-transformers  
datasets  
evaluate  
jiwer  
rouge-score  
gradio  
torch  
pandas  
numpy  
scipy

---

## Author

Developed as an Arabic NLP project for audio understanding, summarization, semantic retrieval, and grounded question answering.

---

## License

This project is for academic and educational purposes.
