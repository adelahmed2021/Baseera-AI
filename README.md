# 🧠 Baseera AI - Multimodal Learning Intelligence Microservice

**Baseera AI** is the core Artificial Intelligence microservice powering a comprehensive E-Learning platform. Built to integrate seamlessly with a main Student/Instructor LMS (Java/Spring Boot), this Python-based engine handles asynchronous heavy lifting for multimedia processing, multimodal search, and advanced Retrieval-Augmented Generation (RAG).

---

## 🏗️ System Architecture & Ingestion Pipeline

The system operates as an independent microservice. When an instructor uploads a course playlist, the media takes a dual path:
1. **Cloud Storage:** Media is synced to **Bunny Object Storage** for content delivery.
2. **AI Processing Pipeline (`/process` endpoint):** The media is downloaded locally to trigger the asynchronous AI ingestion pipeline, divided into two concurrent workflows:

### 🎙️ Audio Processing Workflow
* **Ultra-Fast ASR:** Audio is extracted and transcribed using **Deepgram's Whisper API** for high-speed processing.
* **Smart Chunking:** The transcript is intelligently segmented by grouping utterances until they form logical chunks of ~1 minute.
* **Vectorization:** Text chunks are embedded using **LaBSE** (Language-agnostic BERT Sentence Embedding) to capture deep semantic meaning, especially for Arabic dialects.
* **Storage:** Embeddings and metadata are stored in a **PostgreSQL** database utilizing the **pgvector** extension.

### 🖼️ Video/Visual Processing Workflow
* **Frame Extraction:** Video frames are sampled at a rate of 1 frame per second.
* **Semantic Deduplication:** To optimize storage and search speed, frames are evaluated using **CLIP**. Highly similar or duplicate frames are filtered out.
* **Storage:** The remaining unique visual embeddings are stored in PostgreSQL alongside the audio vectors.

---

## 🚀 Core AI Features

### 1. 💬 Context-Aware Chat with Video
Users can ask highly specific questions about the content of any video.
* **Retrieval Flow:** The system receives the user query and `video_id`. It searches the pgvector database using **Cosine Similarity** to find the most relevant audio chunks.
* **Precision Reranking:** Initial results are passed through a **Local Hugging Face Reranker** to ensure the utmost contextual accuracy.
* **Generation:** The reranked context is fed into a high-speed **Open Source 20B LLM via Groq**, guided by a strict system prompt to generate accurate, video-specific answers.

### 2. 🔍 Multimodal Search (Search by Image, Video, or Text)
Users can pinpoint exact moments within a specific course using visual or text inputs.
* **Course-Scoped Querying:** The search is strictly scoped using the `course_id` to ensure high relevance and lower latency.
* **Visual Matching:** When queried with an image or video snippet, the system generates a CLIP embedding and performs a vector search against the deduplicated frames in the database.
* **Timestamp Extraction:** Returns the exact `video_id` and the specific minute index where the visual or spoken concept appears.

### 3. 📚 Intelligent Curriculum RAG (Egyptian Schools)
A specialized RAG pipeline designed for the complex structures of the Egyptian academic curriculum.
* **Hybrid Intent Routing:** To prevent embedding confusion across highly distinct subjects, user queries pass through a dual-layer classifier:
  1. **Rule-Based Classifier**
  2. **LLM-Based Classifier**
* **Targeted Retrieval:** The question is routed to one of **6 specific academic branches** (e.g., Grammar/Nahw, Rhetoric/Balagha, Expression/Ta'beer), ensuring the vector search retrieves data only from the highly relevant academic domain.

---

## 🛠️ Tech Stack

* **Main Platform Ecosystem:** Java, Spring Boot (External Microservices)
* **AI & Logic Microservice:** Python, FastAPI / LangChain
* **Storage & Databases:** PostgreSQL + pgvector, Bunny Object Storage
* **Audio & Speech:** Deepgram (Whisper)
* **Vision & Embeddings:** CLIP, LaBSE (Language-agnostic BERT)
* **LLMs & Inference:** Groq API (OSS 20B Models), Hugging Face (Local Rerankers)

---

## 📦 Installation & Setup

### Prerequisites
* PostgreSQL with `pgvector` enabled.
* Python 3.10+
* API Keys for Groq and Deepgram.

### Getting Started

```bash
# Clone the repository
git clone [https://github.com/adelahmed2021/baseera-ai.git](https://github.com/adelahmed2021/baseera-ai.git)
cd baseera-ai

# Install dependencies
pip install -r requirements.txt


