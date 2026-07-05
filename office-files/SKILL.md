---
name: office-files
description: Creates, reads, and edits Word (.docx), Excel (.xlsx), and PowerPoint (.pptx) files using Python with python-docx, openpyxl, and python-pptx. Use when the user mentions any Office file, wants to generate/modify Word documents, spreadsheets, presentations, or mentions .docx/.xlsx/.pptx — even without explicitly asking for a skill.
---

# Office Files

Handle Word, Excel, and PowerPoint files using Python libraries.

## Libraries

| Format | Library | Install |
|--------|---------|---------|
| `.docx` | `python-docx` | `pip install python-docx` |
| `.xlsx` | `openpyxl` | `pip install openpyxl` |
| `.pptx` | `python-pptx` | `pip install python-pptx` |

Check if installed first: `python3 -c "import docx; import openpyxl; import pptx"` — install only what's missing.

## Workflow

1. Write a Python script to `/tmp/` to create or modify the file
2. Run it with `python3 /tmp/script.py`
3. Tell the user where the output file is

## Quick Reference

**Word (.docx)**
```python
from docx import Document
doc = Document()
doc.add_heading('Title', 0)
doc.add_paragraph('Content here.')
doc.save('/tmp/output.docx')
```

**Excel (.xlsx)**
```python
from openpyxl import Workbook
wb = Workbook()
ws = wb.active
ws['A1'] = 'Header'
ws.append([1, 2, 3])
wb.save('/tmp/output.xlsx')
```

**PowerPoint (.pptx)**
```python
from pptx import Presentation
from pptx.util import Inches, Pt
prs = Presentation()
slide = prs.slides.add_slide(prs.slide_layouts[1])
slide.shapes.title.text = 'Title'
slide.placeholders[1].text = 'Content'
prs.save('/tmp/output.pptx')
```

**Reading existing files:** pass the file path to `Document(path)`, `load_workbook(path)`, or `Presentation(path)`.

## Notes

- Output to `/tmp/` so user can copy to Windows via Samba (`\\nest\hushus` or `\\nest\work`)
- For complex formatting (styles, charts, images), build inline — don't import external scripts
