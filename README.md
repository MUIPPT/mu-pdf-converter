<p align="center">
  <img alt="mu-pdf-converter" src="assets/default-banner.png" width="100%">
</p>

# 🔄 mu-pdf-converter

> High-fidelity PDF format converter — transform PDFs into fully editable PowerPoint, Word, Excel, and Images with four-layer parsing, three-engine table extraction, and automatic foreign language translation.

**English** | [中文](README_CN.md) | [🌐 Landing Page](https://mupoet.github.io/mu-pdf-converter/)

[![WeChat](https://img.shields.io/badge/muippt-07C160?logo=wechat&logoColor=white)](https://mp.weixin.qq.com/s/v1JSZvlN5fvbOOHvkvXEtA)
[![Xiaohongshu](https://img.shields.io/badge/muippt-FF2442?logo=xiaohongshu&logoColor=white)](https://xhslink.com/m/ESxtgUNMdl)
[![Book](https://img.shields.io/badge/Book-Visual%20Team%20Management-BBDDE5?logo=bookstack&logoColor=white)](https://item.m.jd.com/product/14547345.html)
[![License](https://img.shields.io/github/license/mupoet/mu-pdf-converter)](LICENSE)
[![Version](https://img.shields.io/github/v/release/mupoet/mu-pdf-converter)](https://github.com/mupoet/mu-pdf-converter/releases)
[![Stars](https://img.shields.io/github/stars/mupoet/mu-pdf-converter)](https://github.com/mupoet/mu-pdf-converter/stargazers)

### 💡 Usage Examples

- 📊 **Financial Reports** — "Convert this English annual report PDF to PPT with auto-translation to Chinese"
- 📋 **Batch Invoices** — "Scan the entire invoices directory and extract all tables into one Excel file"
- ✏️ **Contract Editing** — "Convert this PDF contract to Word so I can edit a few clauses"
- 🧹 **Watermark Removal** — "This PDF has a distracting watermark, please remove it"
- 📝 **Form Filling** — "Fill out this PDF application form: name=xxx, date=xxx"
- 🖼️ **Archival Images** — "Convert each page of this 20-page PDF to high-res PNG at 300 DPI"
- 🌍 **Paper Translation** — "Convert this English research paper PDF to PPT, preserving layout while translating"
- 📑 **Data Extraction** — "Extract the tables on pages 3-5 of this PDF report into Excel"

### ✨ Core Highlights

#### 🔄 Four-Layer PPT Conversion

Not a simple "screenshot pasted into PPT" — four independent parsing layers are precisely stacked, ensuring every element in the output is editable:

| Layer | Content | Result |
|-------|---------|--------|
| Layer 1 | Bitmaps (PNG/JPEG) | Original image clarity and format preserved |
| Layer 2 | Vectors (SVG) | Directly editable paths and shapes in Office 2019+ |
| Layer 3 | Tables (Native Table) | Rows/columns editable, supports formatting changes |
| Layer 4 | Text Boxes (TextBox) | Editable text with font/color/bold/italic preserved |

> Final effect: Open the PPT and text is selectable, tables can add/remove rows, shapes can scale/rotate — not a "dead image."

#### 📊 Three-Engine Table Extraction

Complex tables break single-engine approaches. mu-pdf-converter uses a cascading three-engine strategy with automatic fallback:

| Priority | Engine | Strength |
|----------|--------|----------|
| E1 | XY-Cut (pypdfium2) | Borderless tables, complex merged cells |
| E2 | MarkItDown (AI-assisted) | Non-standard layouts, loosely aligned data |
| E3 | pdfplumber | Standard bordered tables, compatibility fallback |

> E1 fails → tries E2 → E2 fails → falls back to E3. Three-tier safety net ensures you never come back empty-handed.

#### 🌍 Automatic Foreign Language Translation

Automatically detects foreign language content during PDF-to-PPT conversion — no manual steps needed:

- **Smart detection**: CJK character ratio < 5% triggers auto-translation
- **Term protection**: Technical terms, brand names, code snippets are preserved untouched
- **Dual-engine**: translators library (Google/Baidu/Alibaba cascade) → local LLM (Ollama / OpenAI-compatible) fallback
- **Batch merging**: Reduces API calls for large documents, dramatically improving throughput

#### 📝 Dual-Path Form Filling

Supports two PDF form scenarios, covering all common form types:

| Path | Use Case | Method |
|------|----------|--------|
| Path A: Field Fill | Standard PDFs with fillable form fields | Parses AcroForm fields, writes values precisely |
| Path B: Coordinate Annotation | "Flat" PDFs without fillable fields | AI analyzes page images → overlays text at coordinates |

> Path B is perfect for PDFs that "look like forms but are actually flat images" — converts to image for analysis, then annotates precisely.

#### 🧹 4 Watermark Removal Strategies

Not just simple pattern matching — multiple strategies combined to handle all watermark types:

| Strategy | Description |
|----------|-------------|
| Text Mode | Identifies and removes rotated/semi-transparent text watermarks |
| Image Mode | Detects and removes image-based watermarks (logos/stamps) |
| XObject Mode | Cleans watermark references in PDF internal XObject layer |
| Aggressive Mode | Combines all strategies + heuristic rules for stubborn watermarks |

> Supports `--detect-only` preview mode — see what's detected before committing to removal.

#### 🔌 MCP Server (Model Context Protocol)

Built-in MCP Server enables seamless integration with any AI assistant:

- Protocol: JSON-RPC 2.0 over stdin/stdout
- Zero external dependencies: RPC layer uses only Python standard library
- Exposes 6 tools: `pdf_to_pptx` / `pdf_to_docx` / `pdf_to_xlsx` / `pdf_to_images` / `pdf_fill_form` / `pdf_remove_watermark`

### 📌 Comparison

| **Dimension** | **mu-pdf-converter** | **Adobe Acrobat** | **SmallPDF** | **iLovePDF** | **pdf2docx** |
|---|---|---|---|---|---|
| PDF→PPT editability | ✅ 4-layer, text/tables/shapes all editable | ✅ Good | ⚠️ Partial | ⚠️ Partial | ❌ Not supported |
| Table extraction engine | Three-engine cascade (XY-Cut→MarkItDown→pdfplumber) | Single | Single | Single | Single |
| Auto-translation | ✅ Translate + preserve layout | ❌ None | ❌ None | ❌ None | ❌ None |
| Batch processing | ✅ Directory scan, 100-file batches | ✅ Yes | ⚠️ Limited | ⚠️ Limited | Manual coding |
| Form filling | ✅ Dual-path (field fill + coordinate annotation) | ✅ Yes | ❌ No | ❌ No | ❌ No |
| Watermark removal | ✅ 4 strategies | ✅ Yes | ❌ No | ⚠️ Limited | ❌ No |
| MCP Server | ✅ Built-in | ❌ No | ❌ No | ❌ No | ❌ No |
| Local/Privacy | ✅ 100% local | ⚠️ Cloud features | ❌ Upload to cloud | ❌ Upload to cloud | ✅ Local |
| Open source | ✅ MIT | ❌ Commercial | ❌ Closed | ❌ Closed | ✅ Open |
| Cost | Free | $239.88/yr | $9-18/mo | $7-12/mo | Free |

### 🚀 Workflows

| Workflow | Scenario | Trigger |
|----------|----------|---------|
| PDF → PPT | High-fidelity 4-layer conversion with auto-translation | "Convert PDF to PPT", "PDF to PPTX" |
| PDF → Word | Text paragraphs + heading detection + table preservation | "Convert PDF to Word", "Make this PDF editable" |
| PDF → Excel | Three-engine table extraction, batch directory scanning | "Extract PDF tables", "Batch extract tables" |
| PDF → Images | High-quality per-page rendering, custom DPI | "Convert PDF to images", "PDF to PNG" |
| Form Filling | Dual-path: fillable fields / coordinate annotation | "Fill PDF form", "PDF form filling" |
| Watermark Removal | 4 strategies with preview mode | "Remove PDF watermark", "Remove watermark" |
| Translation | Auto-translates foreign content during PDF→PPT | "Translate foreign PDF", "Translate English PDF to Chinese" |

### ⚙️ Technical Specs

| Item | Description |
|------|-------------|
| Runtime | Python 3.9+ |
| PDF parsing | PyMuPDF (fitz) + pdfplumber |
| PPT generation | python-pptx + lxml (SVG insertion) |
| Word generation | python-docx |
| Excel generation | openpyxl |
| Table detection | XY-Cut v2 (pypdfium2) → MarkItDown → pdfplumber |
| Translation | translators (multi-engine) + local LLM (OpenAI-compatible API) |
| MCP protocol | JSON-RPC 2.0 over stdin/stdout, zero external deps |
| Batch limit | ≤ 100 PDFs per batch, ≤ 5000 text blocks per translation |

### 🛠️ Quick Start

**Step 1: Install dependencies**

```bash
# Core (required)
pip install pymupdf pdfplumber python-pptx python-docx openpyxl lxml pypdf

# Optional (enhanced features)
pip install pypdfium2 markitdown translators requests
```

**Step 2: Convert**

```bash
# PDF → PPT (foreign PDFs auto-translated)
python scripts/pdf_to_pptx.py report.pdf --outfile report.pptx

# PDF → Excel (batch: extract tables from all PDFs in directory)
python scripts/pdf_to_xlsx.py --batch ./invoices/ --outfile batch.xlsx

# PDF watermark removal (preview then execute)
python scripts/pdf_remove_watermark.py doc.pdf --detect-only
python scripts/pdf_remove_watermark.py doc.pdf --outfile clean.pdf
```

**Step 3: MCP Server (optional)**

```bash
python scripts/mcp_server.py
```

> 💡 Core conversion works without optional dependencies. Only XY-Cut table engine and translation require their respective packages.

### 🔒 Security & Privacy

- **100% local processing** — all PDF conversion happens on your machine
- **No cloud uploads** — your files never leave your computer
- **No telemetry** — zero tracking, zero analytics, zero data collection
- **Optional external calls** — only translation may call external APIs (disable with `--no-translate`)
- **MIT License** — fully open source, audit and modify freely

### ⭐ Star History

If this tool saves you time, consider giving it a star ⭐ — it helps others discover it too!

[![Star History Chart](https://api.star-history.com/svg?repos=mupoet/mu-pdf-converter&type=Date)](https://star-history.com/#mupoet/mu-pdf-converter&Date)

> Not "upload to cloud and wait" — local four-layer parsing that brings every element in your PDF back to life.

### 👤 About the Author

🎓 Signatory Author of Tsinghua University Press / 2026 Dangdang Influential Author / AI & Large Model Business HR Specialist at a Leading Tech Company / National Level-1 HR Manager / Level-2 Psychological Counselor / Self-taught Designer

📚 Author of [*Visual Team Management*](https://item.m.jd.com/product/14547345.html). Clients include ByteDance, Tencent, Baidu, China Mobile, SMG, BOE…

💡 [WeChat Official Account](https://mp.weixin.qq.com/s/v1JSZvlN5fvbOOHvkvXEtA) / [Xiaohongshu](https://xhslink.com/m/ESxtgUNMdl): muippt

### 📄 License & Acknowledgments

[MIT](LICENSE) © 2024-2026 mupoet (木先生iPPT)

**Acknowledgments**: Built upon the excellent work of [PyMuPDF](https://github.com/pymupdf/PyMuPDF), [python-pptx](https://github.com/scanny/python-pptx), [pdfplumber](https://github.com/jsvine/pdfplumber), [MarkItDown](https://github.com/microsoft/markitdown), and the open-source community.

> Note: Much of this project was co-created with AI assistance. If you believe your work has been used without proper attribution, please open an issue.
