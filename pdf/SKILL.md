---
name: pdf
description: Reads and extracts text, tables, and data from PDF files using Python. Use when the user mentions a .pdf file or wants to extract information from a PDF.
---

# PDF — Read & Extract

## Library

```bash
pip install pypdf
```

Check first: `python3 -c "import pypdf"` — install only if missing.

## Extract text

```python
from pypdf import PdfReader

reader = PdfReader("document.pdf")
text = "\n".join(page.extract_text() for page in reader.pages)
print(text)
```

## Extract specific pages

```python
reader = PdfReader("document.pdf")
page = reader.pages[0]  # 0-indexed
print(page.extract_text())
```

## If pypdf gives poor results (scanned/image PDF)

Use `pdfplumber` — better for tables and complex layouts:

```bash
pip install pdfplumber
```

```python
import pdfplumber

with pdfplumber.open("document.pdf") as pdf:
    for page in pdf.pages:
        print(page.extract_text())
        # Tables:
        tables = page.extract_tables()
        for table in tables:
            for row in table:
                print(row)
```

## Scanned PDF (no text layer)

```bash
sudo apt install tesseract-ocr poppler-utils
pip install pytesseract pdf2image
```

```python
from pdf2image import convert_from_path
import pytesseract

images = convert_from_path("scan.pdf")
text = "\n".join(pytesseract.image_to_string(img) for img in images)
print(text)
```

## Workflow

1. Write script to `/tmp/extract.py`, run with `python3 /tmp/extract.py`
2. For large PDFs, extract page by page and filter what's needed
3. Output to `/tmp/output.txt` so user can read it
