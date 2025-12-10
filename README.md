# RAG with OpenAI: Turning a Cookbook into a Searchable AI Knowledge System

This repo contains my final notebook from a RAG (Retrieval-Augmented Generation) course, where I built an end-to-end pipeline that takes a scanned cookbook PDF and turns it into a **semantically searchable recipe knowledge base**.

I used this project to deepen my skills as an **AI Solutions Engineer**: connecting unstructured data, embeddings, vector search, and LLMs into a working system.

---

## What the Notebook Does

The main file in this repo is:

- `RAG_with_OpenAI.ipynb`

Inside that notebook, the pipeline:

1. **Converts a PDF into images**  
   - Uses `pdf2image` and `poppler-utils` to turn each page of _"Things Mother Used to Make"_ into `.jpg` images.

2. **Extracts structured recipes from each page with GPT-4o**  
   - Sends each image to a multimodal OpenAI model (`gpt-4o-mini` in my case).  
   - Asks the model to return structured recipe data:
     - recipe title  
     - ingredients  
     - instructions  
     - tags / dish type (when present)

3. **Filters to keep only real recipes**  
   - Drops pages that don’t contain recipe-like content (no “ingredients”, “instructions”, etc.).  
   - Saves the cleaned recipe data to `recipe_info.json`.

4. **Generates embeddings for each recipe**  
   - Uses `text-embedding-3-large` to create a dense vector embedding for each recipe.  
   - Collects embeddings into an `embedding_matrix` (NumPy array).

5. **Builds a FAISS index for semantic search**  
   - Adds all embeddings to a FAISS `IndexFlatL2`.  
   - Saves:
     - the index → `filtered_recipe_index.index`  
     - the associated metadata (recipe text + image path) → `filtered_recipe_metadata.json`

6. **Prepares for querying (RAG-style)**  
   - Sets up the pieces needed to:
     - embed a natural-language query  
     - search the FAISS index  
     - retrieve the most relevant recipes  
   - This is the retrieval backbone you’d plug into a chat-style RAG system.

---

## Why This Matters (Beyond Recipes)

The dataset is playful, but the pattern is very real:

- **Support & Success**  
  - Turn PDFs, guides, and knowledge base articles into a searchable system grounded in company-approved content.
- **Presales & Solutions Engineering**  
  - Surface relevant case studies, diagrams, or integration docs based on a prospect’s question.
- **Internal Knowledge Search**  
  - Let teams query long-form docs (SOPs, policies, reports) using natural language instead of Ctrl+F.

This project demonstrates how to:

- Use LLMs for **document understanding**, not just chat  
- Build **embeddings + vector search** with FAISS  
- Set up the core of a **Retrieval-Augmented Generation** workflow

---

## Tech Stack

- **Python**
- **OpenAI API**
  - `gpt-4o-mini` for image + text extraction
  - `text-embedding-3-large` for embeddings
- **FAISS** (`faiss-cpu`) for vector similarity search
- **pdf2image** + `poppler-utils` for PDF → image conversion
- **NumPy** / `json` / `os` for data handling

---

## How to Run It

### 1. Clone the repo

```bash
git clone https://github.com/<your-username>/rag-with-openai-recipes.git
cd rag-with-openai-recipes
