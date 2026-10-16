# 🚗 Vehicle Specification Extraction Pipeline

Semantic Retrieval + LLM-Based Structured Extraction (Single-File Version)

This repository provides an end-to-end single-file Python pipeline for extracting automotive specifications from service manuals (PDFs).

The system uses:

- PDF parsing
- Semantic chunk embeddings (SentenceTransformers)
- FAISS vector search
- LLM-powered JSON extraction (OpenAI GPT)

All logic is contained inside one Python file, making it ideal for academic submissions, demos, interviews, and compact repositories.

## ✨ Key Features

### 🔍 Complete Automated Pipeline

- Extracts text from PDFs using PyMuPDF
- Creates overlapping chunks for context preservation
- Builds embeddings using MiniLM-L6-v2
- Retrieves most relevant chunks for any query
- Runs LLM extraction (JSON output) for:
  - Torque values
  - Fluid capacities
  - Part numbers
  - Any mechanical specification

### 🔐 Runtime API Key Prompt

If `--use_openai` is enabled and no key is found:

> The script securely prompts you to enter your OpenAI API key.

### 🧩 Single Python File

- `vehicle_spec_extraction.py`

No modules, no folders — everything in one place.

## 📦 Requirements

Install dependencies:

```bash
pip install numpy pandas sentence-transformers faiss-cpu pymupdf openai python-dotenv tqdm
```

Windows FAISS wheel:

```bash
pip install faiss-cpu==1.7.4
```

## ▶️ Usage

### 1) Retrieval Only (no LLM required)

```bash
python vehicle_spec_extraction.py manual.pdf "torque for brake caliper bolts"
```

Produces a `.json` file with retrieved text chunks.

### 2) Full LLM Extraction

```bash
python vehicle_spec_extraction.py manual.pdf \
  "torque for brake caliper bolts" \
  --use_openai \
  --out specs.json
```

If the API key isn't set, the script asks:

```text
Paste your OpenAI API key (it will not be shown):
```

## 📤 Output Example (JSON)

```json
[
  {
    "component": "brake caliper bolt",
    "spec_type": "torque",
    "value": 35,
    "unit": "N·m",
    "source_page": 22,
    "chunk_id": "p22_c0"
  }
]
```

To export CSV:

```bash
--out_csv specs.csv
```

## ⚙️ CLI Options

| Option | Description |
|---|---|
| `--use_openai` | Enables LLM extraction |
| `--chunk_size` | Chunk size (default 1000 chars) |
| `--overlap` | Chunk overlap (default 200 chars) |
| `--top_k` | Retrieved chunks (default 6) |
| `--emb_model` | Embedding model |
| `--openai_model` | GPT model |
| `--out` | Output JSON path |
| `--out_csv` | Output CSV path |
| `--verbose` | Detailed logs |

## 🧠 Pipeline Architecture

PDF → Text

Text → Overlapping Chunks

Chunks → Embeddings

FAISS Retrieval

LLM Structured Extraction

JSON / CSV Output
