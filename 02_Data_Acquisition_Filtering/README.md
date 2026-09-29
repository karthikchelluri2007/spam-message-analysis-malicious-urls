# Stage 2: Data Acquisition & Filtering

Keep only the needed columns, drop missing values, normalise text, keep the four valid classes, and remove empty URLs and zero-width/BOM characters.

**Notebook:** `02_data_acquisition_filtering.ipynb`

Run the notebooks in stage order (01 to 08). Each one reads the previous stage's output from `dataset/intermediate/` and writes its own output.
