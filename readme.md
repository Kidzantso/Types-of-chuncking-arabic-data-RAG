# Arabic RAG Chunking Evaluation

This repository contains a Jupyter/Colab notebook for evaluating different chunking and retrieval strategies on Arabic text extracted from a PDF copy of *Atomic Habits*.

The notebook starts from an image-based Arabic PDF, extracts text with OCR, cleans and normalizes the Arabic text, then compares multiple Retrieval-Augmented Generation (RAG) approaches using LangChain, Chroma, Sentence Transformers, and a local Ollama LLM.

## Files

- `RAG_evaluation_using_Arabic_data.ipynb` - main notebook.
- `www.norbook.shop ... .pdf` - source Arabic PDF used by the notebook.
- `readme.md` - project overview and usage notes.

## What the Notebook Does

1. Installs and starts Ollama, then pulls the `llama3` model.
2. Loads the Arabic PDF.
3. Uses OCR with Tesseract Arabic support because the PDF is image-based.
4. Cleans the extracted Arabic text by normalizing Arabic characters, removing punctuation, numbers, tatweel, URLs, emails, mentions, hashtags, emojis, and non-Arabic characters.
5. Builds several chunking strategies:
   - Recursive character text splitting
   - Basic semantic chunking
   - Adaptive semantic chunking
   - Markdown/document-structure chunking
   - Agentic chunking using an LLM as an editor
   - Parent-document retrieval
   - Knowledge graph-based retrieval
6. Creates vector stores with Chroma and embeddings from `all-MiniLM-L6-v2`.
7. Evaluates each retrieval strategy with the same Arabic questions and prompt template.

## Main Technologies

- Python
- Google Colab / Jupyter Notebook
- Tesseract OCR with Arabic language data
- PyMuPDF
- pytesseract
- pypdf
- LangChain
- LangChain Community
- LangChain Ollama
- LangChain Classic
- ChromaDB
- Sentence Transformers
- NetworkX
- Ollama with `llama3`

## Recommended Runtime

The notebook is written for a Colab-style Linux environment. Several cells use commands such as:

```bash
sudo apt install -y tesseract-ocr tesseract-ocr-ara
curl -fsSL https://ollama.com/install.sh | sh
ollama pull llama3
```

If you run it locally on Windows, you will need to install Tesseract, the Arabic language pack, Ollama, and Python dependencies manually, then update file paths such as:

```python
pdf_path = "/content/<source-arabic-pdf>.pdf"
```

## How to Run

1. Open `RAG_evaluation_using_Arabic_data.ipynb` in Google Colab or Jupyter.
2. Make sure the PDF file is available at the path expected by the notebook.
3. Run the setup cells to install dependencies, Tesseract OCR, and Ollama.
4. Run the OCR and preprocessing sections.
5. Run each chunking strategy section.
6. Run the evaluation sections to compare answers produced by each RAG pipeline.

## Evaluation Questions

The notebook uses Arabic questions about the book, including:

- What are atomic habits and why are they important?
- How can a person build good habits according to the author?
- What is the importance of environment in forming habits?
- Does the book provide specific strategies for changing bad habits?

Each retrieval method is tested against the same questions so the outputs can be compared more fairly.

## Notes

- OCR quality strongly affects the downstream RAG results. If the extracted Arabic text contains encoding or recognition errors, retrieval and generation quality will drop.
- The markdown-header chunking section uses a small sample markdown text rather than the full OCR output.
- Agentic chunking is demonstrated on a small slice of the cleaned text to avoid many LLM calls.
- The notebook uses `all-MiniLM-L6-v2`, which is multilingual enough for experimentation but may not be optimal for Arabic retrieval. For stronger Arabic results, consider testing Arabic or multilingual embedding models.
- Chroma collections are deleted after several evaluation runs to free resources.

## Project Goal

The goal is to compare how chunking choices affect Arabic RAG quality. The notebook is useful as an experimental baseline for Arabic document retrieval, OCR-based pipelines, and chunking strategy evaluation.
