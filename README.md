# Docling Conversion Tutorials

A collection of tutorials and techniques for document processing and parsing using [Docling](https://docling-project.github.io/docling/) and related tools.

This repository provides a curated set of opinionated _conversion profiles_ that, based on our experience, effectively address common problems users face when preparing documents for AI ingestion. Our goal is to help users achieve great results without needing to deeply understand all the options and features that Docling offers.


```bash
pip install docling
```

## Default settings

```bash
docling /path/to/document.pdf --to md
```

## Force OCR

```bash
docling /path/to/document.pdf --to md --ocr-engine easyocr --force-ocr --ocr-lang en

```

## VLM

```bash
docling /path/to/document.pdf --to md --pipeline vlm --vlm-model granite_vision_ollama

```

## VLM with Ollama, no Docling

```python
import ollama
from PIL import Image
import base64
import io
import time

img = Image.open('/home/ffranz/Dev/e2e-poc-source-documents/safebalance-clarity-statement-png/safebalance-clarity-statement-1.png')
buffer = io.BytesIO()
img.save(buffer, format="PNG")
img_base64 = base64.b64encode(buffer.getvalue()).decode('utf-8')
msg = {
    "role": "user",
    "content": f"""Extract **all text** from the image you received, exactly as it appears. **No modification, summarization, or omission**.
    Format the output as markdown, using up to six levels of headers (#, ##, ###, ####, #####, and ######) only where they appear in the image, preserving bulleted and numbered lists, and maintaining basic text formatting (bold, italic, underline) exactly where they appear.
    """,
    "images": [img_base64]
}
response = ollama.chat(model='granite3.2-vision', messages=[msg])
print(response['message']['content'])
```

## Other options

### Image export mode

## In case you prefer a web UI

```
pip install "docling-serve[ui]"
docling-serve run --enable-ui
```