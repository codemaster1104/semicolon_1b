# 🚀 Semicolon 1B: Intelligent PDF Section Extractor

Unlock the power of semantic search and advanced PDF processing with our robust, production-ready pipeline! Designed for speed, accuracy, and ease of use, this solution leverages state-of-the-art NLP models and is fully containerized for effortless deployment anywhere.

---

## ✨ Features

- 🔍 **Semantic section extraction** using Sentence Transformers
- 📄 Handles complex, multi-document PDF collections
- 🐳 **Dockerized** for zero-hassle setup and reproducibility
- ⚡ Fast, scalable, and easy to extend
- 🛠️ Clean, modular Python codebase
- 💡 Plug-and-play: just mount your data and run!

---

## 🏗️ Architecture Overview

1. **Input**: JSON file describing persona, job, and a list of PDF documents.
2. **Processing**:
   - Extracts headings and section texts from each PDF.
   - Embeds query and sections using state-of-the-art sentence transformers.
   - Ranks sections by semantic similarity to the user’s task.
3. **Output**: JSON file with the most relevant sections and refined text for further analysis.

```
[Input JSON + PDFs] → [Section Extraction] → [Embedding & Ranking] → [Output JSON]
```

---

## 🚀 Quickstart (Docker)

### 1. Build the Docker image

From your project root:
```sh
docker build -t semicolon1b-pipeline -f Challenge_1b/Dockerfile Challenge_1b
```

### 2. Run the pipeline

Mount your data directory and run:
```sh
docker run --rm -v "$PWD/Challenge_1b/Collection 1:/data" semicolon1b-pipeline "/data/challenge1b_input.json" "/data/PDFs" "/data/pipeline_output.json" 5
```
- Adjust the mount and arguments for other collections as needed.

---

## 📥 Input / 📤 Output Format

### Input JSON Example

```json
{
  "persona": { "role": "Researcher" },
  "job_to_be_done": { "task": "Find key insights on French cuisine" },
  "documents": [
    { "filename": "South of France - Cuisine.pdf" },
    { "filename": "South of France - History.pdf" }
  ]
}
```

- **PDFs Directory**: Place all referenced PDFs in the specified directory (e.g., `/data/PDFs`).

### Output JSON Example

```json
{
  "metadata": {
    "input_documents": ["South of France - Cuisine.pdf", "South of France - History.pdf"],
    "persona": "Researcher",
    "job_to_be_done": "Find key insights on French cuisine",
    "processing_timestamp": "2025-07-28T16:00:00"
  },
  "extracted_sections": [
    {
      "document": "South of France - Cuisine.pdf",
      "section_title": "Traditional Dishes",
      "importance_rank": 1,
      "page_number": 3
    }
  ],
  "subsection_analysis": [
    {
      "document": "South of France - Cuisine.pdf",
      "refined_text": "Bouillabaisse is a classic seafood stew...",
      "page_number": 3
    }
  ]
}
```

---

## ⚙️ How It Works

Our pipeline is designed for robust, accurate, and scalable semantic section extraction from PDFs. Here’s a step-by-step breakdown of the logic:

1. **Input Parsing**  
   The pipeline starts by reading a JSON file that specifies the persona, job/task, and a list of PDF filenames. All referenced PDFs are expected to be present in the specified directory.

2. **Section Extraction**  
   For each PDF:
   - The pipeline runs a heading extraction module (`i3.py`) to identify the document’s structure and outline.
   - It then runs a section text extractor (`section_text_extractor.py`) to pull out the full text of each section, preserving the logical document hierarchy.

3. **Query Construction & Embedding**  
   - The persona and job/task are combined into a single query string (e.g., "Researcher Find key insights on French cuisine").
   - The query is embedded into a high-dimensional vector using the `sentence-transformers/all-MiniLM-L6-v2` model.

4. **Section Embedding**  
   - Each extracted section’s text is also embedded using the same transformer model, producing a vector for every section.

5. **Semantic Similarity & Ranking**  
   - The pipeline computes the cosine similarity between the query embedding and each section embedding.
   - All sections are ranked by similarity score, identifying which parts of the documents are most relevant to the user’s intent.

6. **Top-N Selection**  
   - The top N sections (as specified by the user) are selected as the most relevant.
   - For each, the pipeline records the document name, section title, page number, and the full section text.

7. **Output Generation**  
   - The results are written to an output JSON file, including metadata, a ranked list of extracted sections, and a refined analysis of each top section.

**Modular Design:**  
Each step is implemented as a standalone Python module or function, making the pipeline easy to maintain, extend, or adapt to new requirements.

This logic ensures that users get the most contextually relevant information from large, complex PDF collections with minimal effort.

---

## 💪 Why Use This Solution?

- **Production-ready**: Fully containerized, works out-of-the-box on any machine with Docker.
- **State-of-the-art NLP**: Harnesses the power of modern transformer models for deep semantic understanding.
- **Scalable**: Handles large document collections and complex queries with ease.
- **Extensible**: Modular design makes it easy to add new features or adapt to new domains.
- **Zero setup pain**: No dependency hell—just build and run!

---

## 👥 Contributors & Acknowledgements

- Developed by the team semicolon
- Built with open-source libraries: [PyMuPDF](https://pymupdf.readthedocs.io/), [sentence-transformers](https://www.sbert.net/), [transformers](https://huggingface.co/transformers/)
- Inspired by the need for smarter, faster document analysis
