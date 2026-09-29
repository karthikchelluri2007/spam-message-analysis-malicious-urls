# Stage 3: Data Extraction

Parse each URL (text only, URLs are never opened) and extract structural and lexical features such as length, domain, TLD, HTTPS, IP address, special characters and suspicious keywords.

**Notebook:** `03_data_extraction.ipynb`

Run the notebooks in stage order (01 to 08). Each one reads the previous stage's output from `dataset/intermediate/` and writes its own output.
