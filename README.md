# GashissDB Dataset Downloader

This repository provides a simple Python script to automatically download the GashissDB dataset from Figshare directly into Google Colab or any Python environment.
The script leverages the Figshare REST API to fetch metadata and download all dataset files with ease.

# Features

Directly downloads the GashissDB dataset from Figshare
Works in Google Colab, Jupyter Notebook, or local Python
Minimal dependencies (requests, pathlib)
Supports chunked downloads for large files
Skips files without direct URLs to avoid errors
Prints progress updates for each download

# Run the Script
```python
import requests
from pathlib import Path
ARTICLE_ID = 15066147  # GashissDB Figshare ID
BASE_URL = "https://api.figshare.com/v2"
metadata = requests.get(f"{BASE_URL}/articles/{ARTICLE_ID}/files").json()
print("Files in article:", [f['name'] for f in metadata])
for f in metadata:
    download_url = f.get("download_url")
    if not download_url:
        print(f"Skipping {f['name']} (no direct URL)")
        continue
    print(f"Downloading {f['name']}...")
    resp = requests.get(download_url, stream=True)
    resp.raise_for_status()
    out_path = Path(f['name'])
    with open(out_path, "wb") as out:
        for chunk in resp.iter_content(chunk_size=8192):
            out.write(chunk)
    print(f"✅ Saved to {out_path}")
```
# Dataset Information
Dataset: GasHisSDB
Host: Figshare
ARTICLE_ID: 15066147

# License
This repository provides only the downloader script.
Please review the dataset’s license and citation requirements on Figshare before use.

# How to cite

You can use the following reference to cite the dataset:

Hu, W., Li, C., Li, X., Rahaman, M. M., Ma, J., Zhang, Y., Chen, H., Liu, W., Sun, C., Yao, Y., Sun, H., & Grzegorzek, M. GasHisSDB: A New Gastric Histopathology Image Dataset for Computer Aided Diagnosis of Gastric Cancer. DOI: 10.6084/m9.figshare.15066147 (2021)
