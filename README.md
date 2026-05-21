# 👁️ Baseera AI - Multimodal Video & Regional RAG Intelligence

**Baseera AI** is an advanced Multimodal AI platform designed to interact, search, and extract deep insights from videos and regional textual content. By combining state-of-the-art Audio-Visual models with robust vector databases, Baseera breaks language and media barriers, fully supporting both Modern Standard Arabic (MSA) and the Egyptian colloquial dialect.

---

## 🚀 Key Features

### 1. 🎥 Chat with Video
* **How it works:** Leveraging **OpenAI's Whisper** for ultra-accurate speech-to-text transcription, the audio is converted into text chunks, indexed, and made queryable.
* **LLM Reasoning:** Powered by **Groq** (for lightning-fast inference) and **Cohere API**, users can contextually chat with any video, ask for summaries, or pinpoint specific topics discussed.
* **Language Support:** Seamlessly understands both formal academic language and local slang nuances.

### 2. 🔍 Multimodal Video Search (Image & Video Queries)
* **How it works:** Video frames are sampled and embedded into a high-dimensional vector space using the **CLIP (Contrastive Language-Image Pre-training)** Transformer model.
* **Visual Matching:** Users can upload an image or another video snippet to search *inside* the target video. The system finds the exact timestamps where the visual match or conceptual similarity occurs.

### 3. 📚 Chat with Curriculum (Advanced RAG)
* **How it works:** A specialized Retrieval-Augmented Generation (RAG) pipeline built with **LangChain**. Academic curricula (PDFs/Documents) are processed, chunked, and embedded.
* **Dialect Adaptation:** Fine-tuned logic to answer students' questions accurately, whether they ask in formal phrasing or casual, everyday dialect.

---

## 🛠️ Tech Stack & Architecture

The project is built with a highly scalable, production-grade architecture:

* **Backend Framework:** Python
* **Orchestration:** LangChain (Managing RAG pipelines, prompts, and chains)
* **LLMs & Embeddings:** Groq (Inference) + Cohere API + Transformers (CLIP, Whisper)
* **Database & Vector Search:** PostgreSQL with **pgvector** extension (Storing relational metadata alongside high-dimensional embeddings)
* **ORM:** SQLAlchemy (Clean data modeling and migrations)

```text
[User Query] ──> [LangChain] ──> [CLIP / Whisper] ──> Embedding Vector
                                                            │
                                                            ▼
[Groq / Cohere] <── [Context Retrieved] <── [PostgreSQL + pgvector]
