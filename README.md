# GitHub Upload Package

Eight notebooks are split by project stage. The embedded tables, text, and chart outputs come from the supplied executed simplified notebook. The raw counts in those outputs match the provided CSV: 651,991 rows, 329 missing URLs, and 345 missing labels.

## Upload and run

Upload the eight stage folders to the repository root. Keep or place the input CSV under `dataset/dataset.csv` or `dataset/malicious_phish_corrupted.csv`. The notebooks also check for the CSV in the repository root and `/content/` (Google Colab).

Open each notebook from its own folder and run them in numeric order. Stages 2–4 save intermediate files under `dataset/intermediate/`; stages 5–8 read the cleaned file from Stage 4. Stage 7 contains the executed charts. Stage 8 saves `results/malicious_url_enriched.csv`.

Dependencies: Python 3, Pandas, NumPy, and Matplotlib. The CSV is not included in this package; use the dataset already in your repository or upload it to the path above.
