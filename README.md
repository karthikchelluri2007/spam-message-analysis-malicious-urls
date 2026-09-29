# Spam Message Analysis Using Malicious URLs

Capstone Project, Review 2

## Overview
Spam messages often carry links, and malicious URLs (phishing, malware, defacement) are one of the main ways spam causes harm. This project studies malicious URLs in depth: it cleans a large labelled URL dataset, extracts structural and lexical features from every URL, analyses how those features differ between benign and malicious classes, and trains a Random Forest classifier to predict the URL type.

The URL analysis and model built here are designed to be applied to the URLs found inside spam messages (see [Next Steps](#next-steps)).

## Repository Structure

```
Spam-Message-Analysis-Using-Malicious-URLs/
│
├── 01_Data_Loading_Reading/            01_data_loading_reading.ipynb
├── 02_Data_Acquisition_Filtering/      02_data_acquisition_filtering.ipynb
├── 03_Data_Extraction/                 03_data_extraction.ipynb
├── 04_Data_Validation_Cleaning/        04_data_validation_cleaning.ipynb
├── 05_Data_Aggregation_Representation/ 05_data_aggregation_representation.ipynb
├── 06_Data_Analysis/                   06_data_analysis.ipynb
├── 07_Data_Visualization/              07_data_visualization.ipynb
├── 08_Results_Interpretation/          08_results_interpretation.ipynb
│
├── dataset/
│   ├── dataset.csv                     raw dataset
│   └── intermediate/                   files passed from one stage to the next
│
├── results/
│   ├── graphs/                         all charts (PNG)
│   └── output files                    metrics.json, classification_report.txt, etc.
│
├── requirements.txt
└── README.md
```

Each stage notebook reads the output of the previous stage from `dataset/intermediate/` and saves its own output there, so the stages must be run **in order (01 to 08)**.

## Project Pipeline
The notebook follows 8 stages:

| Stage | Description |
|-------|-------------|
| 1. Loading | Read the CSV, check shape, columns, data types, class distribution, and run a **data quality audit** on the raw data |
| 2. Acquisition & Filtering | Keep `url` and `type`, drop missing values, normalise text, keep only valid labels (`benign`, `phishing`, `defacement`, `malware`), remove empty URLs and zero-width/BOM characters |
| 3. Extraction | Parse each URL and extract features (see below). URLs are only parsed as text, never opened |
| 4. Validation & Cleaning | Remove duplicate URLs, check domain extraction failures, flag length outliers (IQR rule), re-run the audit on the cleaned data |
| 5. Aggregation | Overall summary and per-class aggregation of the features |
| 6. Analysis | Correlation matrix, chi-square test (`has_ip_address` vs `type`), Random Forest classifier, feature importance |
| 7. Visualization | 7 charts (top TLDs, URL length by class, HTTPS by class, suspicious keywords, correlation heatmap, length vs digits scatter, confusion matrix) |
| 8. Results & Interpretation | Key findings, limitations, next steps |

## Dataset

**Malicious URLs dataset** (`malicious_phish_corrupted.csv`)

| Item | Details |
|------|---------|
| Rows (raw) | 651,991 |
| Columns | `url`, `type` |
| Classes | `benign`, `phishing`, `defacement`, `malware` |
| File | `dataset/dataset.csv` |
| Source | [Kaggle Malicious URLs dataset](https://www.kaggle.com/datasets/sid321axn/malicious-urls-dataset) (the included CSV is a deliberately corrupted copy) |

The file is a deliberately **corrupted** version of the malicious URL dataset, so it contains problems that Stages 1 to 4 detect and fix:
- Missing values in `url` and `type`
- Duplicate URLs
- Inconsistent label case (for example `BeNiGn`, `BENIGN`, `Benign`)
- Invalid or garbage labels (for example `-`, `NONE`, `123`)
- Whitespace, empty URLs, extremely long URLs, and zero-width/BOM characters

## Extracted Features
| Feature | Meaning |
|---------|---------|
| `url_length`, `path_length`, `query_length` | Lengths of the whole URL, path and query |
| `domain`, `tld`, `subdomain_count` | Registered domain, top-level domain, number of subdomains |
| `has_https` | 1 if the URL uses HTTPS |
| `has_ip_address` | 1 if the hostname is an IP address |
| `dot_count`, `hyphen_count`, `digit_count`, `special_char_count` | Character counts |
| `has_at_symbol` | 1 if the URL contains `@` |
| `has_shortener` | 1 if the host is a known URL shortener (bit.ly, tinyurl.com, etc.) |
| `suspicious_keyword`, `keyword_count` | Suspicious words found (login, verify, secure, account, bank, update, free, confirm, password, signin, wallet, payment, bonus, support) |
| `file_extension` | File extension in the URL path |

## Technologies Used
- Python 3
- pandas, numpy
- scikit-learn (Random Forest, metrics)
- scipy (chi-square test)
- tldextract (offline suffix list)
- matplotlib, seaborn
- Jupyter Notebook / Google Colab

## How to Run
1. Clone the repository:
   ```bash
   git clone <your-repo-link>
   cd Spam-Message-Analysis-Using-Malicious-URLs
   ```
2. Install the requirements:
   ```bash
   pip install -r requirements.txt
   ```
3. Start Jupyter and open the notebooks **one by one, in order (01 to 08)**. Open each notebook from inside its own folder, because the paths are relative to the project root. Run all cells in each notebook.
   ```bash
   jupyter notebook
   ```
4. Outputs: charts go to `results/graphs/`, metrics and reports go to `results/`, and the enriched dataset is saved as `results/malicious_url_enriched.csv`.

## Results

All eight notebooks were run in order against the included dataset. The executed notebooks contain their outputs, and the generated reports and charts are saved under `results/`.

- Rows after cleaning and duplicate removal: **640,088**
- Random Forest accuracy on the 20% stratified test set: **88.47%** (128,018 test rows)
- Top predictive features: **path length, special character count, and dot count**
- Chi-square test for IP-address presence vs. URL type: **χ² = 302,842.09**, statistically significant (**p < 0.001**)

| URL class | Precision | Recall | F1-score |
|---|---:|---:|---:|
| Benign | 0.88 | 0.97 | 0.92 |
| Defacement | 0.89 | 0.80 | 0.84 |
| Malware | 0.99 | 0.77 | 0.87 |
| Phishing | 0.85 | 0.62 | 0.72 |

Generated files include `metrics.json`, `classification_report.txt`, `aggregation_by_type.csv`, `feature_importance.csv`, `model_predictions.csv`, `malicious_url_enriched.csv`, and seven analysis charts plus feature importance under `results/graphs/`.

## Limitations
- Only lexical and structural features are used. There are no WHOIS, domain-age or page-content signals, so new phishing domains with no obvious lexical cues may be missed.
- Class imbalance can inflate overall accuracy.

## Next Steps
- Load the spam message dataset and extract URLs from the messages with a regex.
- Apply the same feature extraction and the trained model to those URLs.
- Add message-level features (`has_url`, `url_count`, `malicious_url_score`) and compare a spam classifier with and without them.
- Try TF-IDF / n-gram features on URL strings and a gradient-boosted model.

## Team Members

| No. | Name | Role |
|-----|------|------|
| 1 | G. Pavan Sai Srikar | Team Lead |
| 2 | K. Hasini | Team Member |
| 3 | M. Tejaswi | Team Member |
| 4 | Ch. Karthik | Team Member |

**Project Guide:** K. Ashok Teja
