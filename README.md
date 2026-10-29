# Malicious URL Analysis

## Project structure

```text
repository-root/
├── 01_Data_Loading_Reading/
│   └── 01_data_loading_reading.ipynb
├── 02_Data_Acquisition_Filtering/
│   └── 02_data_acquisition_filtering.ipynb
├── 03_Data_Extraction/
│   └── 03_data_extraction.ipynb
├── 04_Data_Validation_Cleaning/
│   └── 04_data_validation_cleaning.ipynb
├── 05_Data_Aggregation_Representation/
│   └── 05_data_aggregation_representation.ipynb
├── 06_Data_Analysis/
│   └── 06_data_analysis.ipynb
├── 07_Data_Visualization/
│   └── 07_data_visualization.ipynb
├── 08_Results_Interpretation/
│   └── 08_results_interpretation.ipynb
├── dataset/
│   ├── dataset.csv                  # input CSV; keep existing repository copy
│   └── intermediate/                # created when notebooks run
└── results/                         # created/updated by the notebooks
    └── malicious_url_enriched.csv
```

## Upload and run

Upload the eight numbered stage folders to the repository root. Keep the input CSV at `dataset/dataset.csv` or `dataset/malicious_phish_corrupted.csv`. The notebooks also check for the CSV in the repository root and `/content/` (Google Colab).

Run the notebooks in numeric order, opening each from its own folder. Stages 2–4 save intermediate files under `dataset/intermediate/`; stages 5–8 read the cleaned file from Stage 4. Stage 7 includes executed charts. Stage 8 saves `results/malicious_url_enriched.csv`.

The included notebook outputs match the supplied CSV's raw counts: 651,991 rows, 329 missing URLs, and 345 missing labels. Dependencies: Python 3, Pandas, NumPy, and Matplotlib.
