# A simple RAG system for the KBC Annual Report

An independent learning project that builds a small retrieval-augmented generation (RAG) system over a publicly available KBC Group annual report, and compares two free LLMs on the same retrieved context.

This project is not affiliated with or endorsed by KBC. The report is downloaded from KBC's public website at runtime and is not stored in this repository.

## What it does

1. Downloads the annual report PDF and extracts text page by page (`pdfplumber`).
2. Splits each page into chunks with `semchunk`, sized with the embedding model's own tokenizer so no chunk exceeds the model's input limit.
3. Embeds the chunks with `sentence-transformers/all-MiniLM-L6-v2` and caches the embeddings locally.
4. Retrieves the most similar chunks for a question using cosine similarity.
5. Generates an answer with an LLM via OpenRouter, instructed to use only the retrieved excerpts, cite page numbers, and decline when the answer is not in the excerpts.
6. Compares two models on identical retrieved context, including a negative test.

All code and results are in `kbc_rag_notebook.ipynb`.

## How to run

1. Install the packages: `pip install -r requirements.txt`
2. Create a free OpenRouter account and API key.
3. Create a file named `.env` in the project folder containing: `OPENROUTER_API_KEY=your-key-here`
4. Open the notebook and run all cells.

## Known limitations

See section 7 of the notebook. In short: page footers are not yet removed, some table text is extracted backwards or garbled, and evaluation is so far qualitative.

## Data governance

Only a public document is sent to the external LLM API. Internal or confidential data would require a self-hosted or contractually covered model.
