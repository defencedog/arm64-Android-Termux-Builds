# pdf-cli — Comprehensive Usage Manual

> **Source:** <https://github.com/lgbarn/pdf-cli>  
> **Binary name:** `pdf-cli` (not `pdf`)  
> **License:** MIT  
> **Type:** Single Go binary, no external dependencies

---

## Table of Contents

1. [Overview](#overview)
2. [Installation](#installation)
3. [Global Flags](#global-flags)
4. [Command Reference](#command-reference)
   - [info](#info)
   - [text](#text)
   - [extract](#extract)
   - [merge](#merge)
   - [split](#split)
   - [rotate](#rotate)
   - [reorder](#reorder)
   - [compress](#compress)
   - [encrypt / decrypt](#encrypt--decrypt)
   - [images](#images)
   - [meta](#meta)
   - [watermark](#watermark)
   - [pdfa](#pdfa)
   - [combine-images](#combine-images)
   - [completion](#completion)
5. [Pipelines & stdin/stdout](#pipelines--stdinstdout)
6. [Configuration](#configuration)
7. [OCR](#ocr)
8. [Real-World Tests with resume.pdf](#real-world-tests-with-resumepdf)
9. [Troubleshooting](#troubleshooting)
10. [Known Limitations](#known-limitations)

---

## Overview

pdf-cli is a fast, lightweight, single-binary CLI tool for everyday PDF operations. It is built in Go and supports Linux, macOS, and Windows.

**Key features:**

- **Fast** — single binary, no GUI, parallel processing for large operations
- **Simple** — intuitive commands that do one thing well
- **Secure** — encrypted PDF support with password protection
- **Scriptable** — JSON output for automation (info, meta)
- **Unix-friendly** — stdin/stdout support for pipelines
- **OCR** — extract text from scanned PDFs using built-in WASM Tesseract

**Supported commands:**

| Command | Description | Batch | stdin | stdout |
|---|---|---|---|---|
| `info` | Display PDF info (pages, metadata, encryption) | ✓ | ✓ | – |
| `merge` | Combine multiple PDFs | – | – | – |
| `split` | Split into pages or chunks | – | – | – |
| `extract` | Extract specific pages | – | ✓ | ✓ |
| `reorder` | Reorder, reverse, or duplicate pages | – | ✓ | ✓ |
| `rotate` | Rotate pages 90/180/270 degrees | ✓ | ✓ | ✓ |
| `compress` | Optimize and reduce file size | ✓ | ✓ | ✓ |
| `encrypt` | Add password protection | ✓ | ✓ | ✓ |
| `decrypt` | Remove password protection | ✓ | ✓ | ✓ |
| `text` | Extract text (with OCR for scans) | – | ✓ | – |
| `text` note | Use `--ocr` only for scanned/image PDFs; for text-type PDFs (Word exports, etc.), plain `text` is faster and more accurate | – | – | – |
| `images` | Extract embedded images | – | – | – |
| `combine-images` | Create PDF from images | – | – | – |
| `meta` | View/modify metadata | ✓ | – | – |
| `watermark` | Add text or image watermarks | ✓ | – | – |
| `pdfa` | PDF/A validation and conversion | – | ✓ | ✓ |

---

## Installation

### Via Go

```bash
go install github.com/lgbarn/pdf-cli/cmd/pdf@latest
```

### Pre-built binaries

Download from the [Releases page](https://github.com/lgbarn/pdf-cli/releases) (Linux amd64/arm64, macOS amd64/arm64, Windows amd64).

### From source

```bash
git clone https://github.com/lgbarn/pdf-cli.git
cd pdf-cli
make build
```

### Verify installation

```bash
pdf-cli --version
# pdf-cli version dev  (or the built version)
```

---

## Global Flags

These flags work with every command:

| Flag | Short | Description |
|---|---|---|
| `--verbose` | `-v` | Detailed output |
| `--force` | `-f` | Overwrite without prompting |
| `--progress` | – | Progress bar for long operations (enabled by default) |
| `--password-file` | – | Read password from file (more secure than `--password`) |
| `--dry-run` | – | Preview without making changes |
| `--log-level` | – | `debug`, `info`, `warn`, `error`, `silent` (default: `error`) |
| `--log-format` | – | `text` or `json` |
| `--help` | `-h` | Help for a command |
| `--version` | – | Version info |

---

## Command Reference

### info

Display PDF information: page count, file size, version, encryption status, and metadata.

```bash
# Basic info
pdf-cli info document.pdf

# Machine-readable JSON output
pdf-cli info document.pdf --format json

# Info for multiple files (summary table)
pdf-cli info doc1.pdf doc2.pdf

# Pipe into jq for scripting
pdf-cli info document.pdf --format json | jq '.pages'
```

**JSON output example:**

```json
{
  "file": "/path/to/document.pdf",
  "size": 160378,
  "size_human": "156.62 KB",
  "pages": 4,
  "version": "1.5",
  "encrypted": false,
  "metadata": {
    "author": "Usama Khan",
    "creator": "Microsoft® Word 2016",
    "producer": "Microsoft® Word 2016"
  }
}
```

> **Note:** The help text lists `csv` and `tsv` as supported formats, but only `json` works in practice. `csv` and `tsv` will return "unsupported format" errors.

**Multi-file output:**

```
FILE                            PAGES    VER    SIZE
------------------------------------------------
pages.pdf                           2    1.7  112.10 KB
rotated.pdf                         4    1.7  155.39 KB
```

---

### text

Extract text content from a PDF. Prints to stdout by default; use `-o` to save to a file.

```bash
# Extract all text
pdf-cli text document.pdf

# Extract text from specific pages
pdf-cli text document.pdf -p 1-5 -o chapter1.txt

# Save to file
pdf-cli text document.pdf -o full_text.txt

# Read from stdin
cat document.pdf | pdf-cli text -
```

**With OCR (for scanned/image PDFs only):**

```bash
# Enable OCR with auto-detect backend
pdf-cli text scanned.pdf --ocr

# Force native Tesseract (faster, requires system install)
pdf-cli text scanned.pdf --ocr --ocr-backend native

# Force WASM backend (built-in, ~15MB download on first use)
pdf-cli text scanned.pdf --ocr --ocr-backend wasm

# Multi-language OCR
pdf-cli text scanned.pdf --ocr --ocr-lang eng+fra

# OCR on specific pages
pdf-cli text scanned.pdf --ocr -p 1-3
```

> **⚠️ Important:** OCR is only needed for **scanned/image PDFs**. For **text-type PDFs** (Word exports, digital documents), plain `pdf-cli text doc.pdf` is faster and produces better results. Using `--ocr` on text-type PDFs may produce poor output.

**OCR backends:**

| Backend | Requirements | Speed | Notes |
|---|---|---|---|
| `auto` (default) | — | — | Uses native Tesseract if available, falls back to WASM |
| `native` | System `tesseract` binary + `tessdata` | Fast (parallel) | Recommended for multi-page documents |
| `wasm` | None (downloads tessdata) | Slower (sequential) | Built-in; ~15MB per language on first use |

> **Note:** `text` does not support `--stdout` (unlike other commands). It writes to stdout by default.

---

### extract

Extract specific pages from a PDF into a new file.

```bash
# Extract pages 1–5
pdf-cli extract document.pdf -p 1-5 -o first5.pdf

# Extract non-contiguous pages
pdf-cli extract document.pdf -p 1,3,5,7 -o odds.pdf

# Extract multiple ranges
pdf-cli extract document.pdf -p 1-3,7,10-12 -o selected.pdf

# From stdin to stdout (pipeline)
cat input.pdf | pdf-cli extract - -p 1-5 --stdout > pages.pdf
```

**Page spec syntax:**

- Single pages: `1`, `3`, `5`
- Ranges: `1-5`, `10-15`
- Combined: `1-3,7,10-12`

---

### merge

Combine multiple PDF files into one. Files are merged in the order specified.

```bash
# Merge two files
pdf-cli merge -o combined.pdf file1.pdf file2.pdf

# Merge multiple files
pdf-cli merge -o combined.pdf doc1.pdf doc2.pdf doc3.pdf

# Merge all PDFs in a directory
pdf-cli merge -o all.pdf *.pdf
```

> **Note:** `merge` does **not** support `--password` for encrypted inputs (despite the flag being listed).

---

### split

Split a PDF into multiple files — one per page by default, or into chunks.

```bash
# Split into individual pages
pdf-cli split document.pdf -o output/
# → output/document_1.pdf, output/document_2.pdf, ...

# Split into chunks of N pages
pdf-cli split document.pdf -n 5 -o chunks/
# → chunks/document_1-5.pdf, chunks/document_6-10.pdf, ...
```

---

### rotate

Rotate pages in a PDF by 90, 180, or 270 degrees (clockwise).

```bash
# Rotate all pages 90° clockwise
pdf-cli rotate document.pdf -a 90 -o rotated.pdf

# Rotate only specific pages
pdf-cli rotate document.pdf -a 180 -p 1-5 -o rotated.pdf

# Batch rotate multiple files (adds '_rotated' suffix)
pdf-cli rotate *.pdf -a 90

# From stdin
cat input.pdf | pdf-cli rotate - -a 90 --stdout > output.pdf
```

---

### reorder

Reorder, reverse, or duplicate pages in a PDF.

```bash
# Custom order: move page 5 to position 2
pdf-cli reorder document.pdf -s "1,5,2,3,4" -o reordered.pdf

# Reverse all pages
pdf-cli reorder document.pdf -s "end-1" -o reversed.pdf

# Skip first page
pdf-cli reorder document.pdf -s "2-end" -o skip-first.pdf

# Duplicate a page
pdf-cli reorder document.pdf -s "1,2,2,3" -o with_dup.pdf

# From stdin
cat in.pdf | pdf-cli reorder - -s "end-1" --stdout > reversed.pdf
```

**Sequence syntax:**

| Syntax | Meaning |
|---|---|
| `1,3,5` | Pages 1, 3, 5 in that order |
| `1-10` | Pages 1 through 10 |
| `end` | Last page |
| `end-1` | Second-to-last page (also reverses range) |
| `2-end` | Pages 2 through last |

---

### compress

Optimize and reduce PDF file size.

```bash
# Compress to a new file
pdf-cli compress large.pdf -o smaller.pdf

# Batch compress (adds '_compressed' suffix)
pdf-cli compress *.pdf

# From stdin
cat input.pdf | pdf-cli compress - --stdout > output.pdf

# With progress bar
pdf-cli compress large.pdf -o smaller.pdf --progress
```

> **Note:** Compression gains depend on the source PDF. Well-optimized PDFs may see little reduction.

---

### encrypt / decrypt

Add or remove password protection from PDFs.

```bash
# Encrypt interactively (recommended — password prompted)
pdf-cli encrypt document.pdf -o secure.pdf

# Encrypt with password from file (most secure)
pdf-cli encrypt document.pdf --password-file pass.txt -o secure.pdf

# Encrypt with separate user and owner passwords
pdf-cli encrypt document.pdf --password userpass --owner-password adminpass -o secure.pdf

# Insecure: inline password (warns about ps/history exposure)
pdf-cli encrypt document.pdf --password secret --allow-insecure-password -o secure.pdf

# Decrypt
pdf-cli decrypt secure.pdf --password-file pass.txt -o unlocked.pdf

# Batch decrypt (adds '_decrypted' suffix)
pdf-cli decrypt *.pdf --password-file pass.txt
```

> ⚠️ **Security warning:** The `--password` flag exposes passwords in process listings (`ps aux`), shell history, and system logs. Prefer `--password-file`, the `PDF_CLI_PASSWORD` environment variable, or interactive prompting.

---

### images

Extract embedded images from a PDF.

```bash
# Extract all images
pdf-cli images document.pdf -o images/
# → images/document_1_X0.png, images/document_1_X1.jpg, ...

# Extract images from specific pages
pdf-cli images document.pdf -p 1-5 -o slides/
```

Original image formats (PNG, JPEG, etc.) are preserved.

---

### meta

View or modify PDF document metadata.

```bash
# View metadata
pdf-cli meta document.pdf

# View multiple files
pdf-cli meta doc1.pdf doc2.pdf

# Set metadata fields
pdf-cli meta document.pdf --title "My Document" --author "Jane Doe" -o updated.pdf

# Set multiple fields at once
pdf-cli meta document.pdf \
  --title "Annual Report" \
  --author "John Smith" \
  --subject "Financial" \
  --keywords "report, finance, 2024" \
  -o updated.pdf

# JSON output
pdf-cli meta document.pdf --format json
```

---

### watermark

Add a text or image watermark to a PDF.

```bash
# Text watermark (diagonal across all pages)
pdf-cli watermark document.pdf -t "CONFIDENTIAL" -o marked.pdf

# Text watermark on specific pages
pdf-cli watermark document.pdf -t "DRAFT" -p 1-5 -o draft.pdf

# Image watermark (centered on each page)
pdf-cli watermark document.pdf -i logo.png -o branded.pdf

# Batch watermark (adds '_watermarked' suffix)
pdf-cli watermark *.pdf -t "CONFIDENTIAL"

# Multiple files
pdf-cli watermark doc1.pdf doc2.pdf -t "DRAFT"
```

> Either `-t` (text) or `-i` (image) must be specified.

---

### pdfa

PDF/A validation and conversion (basic — not full ISO compliance).

```bash
# Validate PDF/A compliance
pdf-cli pdfa validate document.pdf

# Validate with specific level
pdf-cli pdfa validate document.pdf --level 1b

# Convert to PDF/A
pdf-cli pdfa convert document.pdf -o archive.pdf

# Convert with level
pdf-cli pdfa convert document.pdf --level 2b -o archive.pdf
```

> **Limitation:** Only basic PDF/A validation and optimization is supported. Font embedding, color profiles, and full ISO 19005 compliance are not available. For comprehensive validation, use [veraPDF](https://verapdf.org/). For full conversion, use Ghostscript or Adobe Acrobat.

---

### combine-images

Create a PDF from multiple images.

```bash
# Combine two images
pdf-cli combine-images photo1.jpg photo2.jpg -o album.pdf

# Combine all PNGs in a directory
pdf-cli combine-images *.png -o scans.pdf

# Specify page size
pdf-cli combine-images scan1.png scan2.png -o doc.pdf --page-size A4
```

Supported input formats: PNG, JPEG, TIFF. Each image becomes one page.

---

### completion

Generate shell completion scripts.

```bash
# Bash
pdf-cli completion bash >> ~/.bashrc

# Zsh
echo 'autoload -U compinit; compinit' >> ~/.zshrc
pdf-cli completion zsh > "${fpath[1]}/_pdf"

# Fish
pdf-cli completion fish > ~/.config/fish/completions/pdf.fish

# PowerShell
pdf-cli completion powershell | Out-String | Invoke-Expression
```

---

## Pipelines & stdin/stdout

Most output-producing commands support `-` as input (stdin) and `--stdout` for binary output. This enables powerful Unix pipelines:

```bash
# Extract pages 1-5, rotate 90°, save
cat input.pdf | pdf-cli extract - -p 1-5 --stdout | pdf-cli rotate - -a 90 --stdout > output.pdf

# Get page count via JSON + jq
curl -s https://example.com/document.pdf | pdf-cli info - --format json | jq '.pages'

# Compress via pipe
curl -s https://example.com/large.pdf | pdf-cli compress - --stdout > compressed.pdf
```

> When using stdin, pdfcpu requires the entire file, so the PDF is temporarily stored on disk.

---

## Configuration

### Config file

pdf-cli reads configuration from (in precedence order):

1. `$XDG_CONFIG_HOME/pdf-cli/config.yaml`
2. `~/.config/pdf-cli/config.yaml`

### Example config

```yaml
defaults:
  verbose: false
  force: false
  progress: true
ocr:
  language: "eng"
  backend: "auto"
```

### Environment variables

All options can be overridden via `PDF_CLI_`-prefixed env vars:

| Env var | Corresponds to |
|---|---|
| `PDF_CLI_VERBOSE` | `--verbose` |
| `PDF_CLI_FORCE` | `--force` |
| `PDF_CLI_PROGRESS` | `--progress` |
| `PDF_CLI_PASSWORD` | `--password` |
| `PDF_CLI_OCR_LANGUAGE` | `--ocr-lang` |
| `PDF_CLI_OCR_BACKEND` | `--ocr-backend` |
| `PDF_CLI_PERF_OCR_THRESHOLD` | OCR parallelization threshold |
| `PDF_CLI_PERF_TEXT_THRESHOLD` | Text extraction parallelization threshold |
| `PDF_CLI_PERF_MAX_WORKERS` | Max parallel workers |

> Environment variables take precedence over config file values.

### Performance tuning

pdf-cli auto-parallelizes:
- File validation: >3 files
- Text extraction: >5 pages
- Native OCR: >5 images

---

## OCR

OCR is triggered with the `--ocr` flag on the `text` command.

### First-time setup

The WASM backend downloads tessdata on first use (~15MB per language). A SHA256 checksum is verified after download. Failed downloads use exponential-backoff retries.

### Languages

```bash
# English only (default)
pdf-cli text scanned.pdf --ocr

# English + French
pdf-cli text scanned.pdf --ocr --ocr-lang eng+fra

# English + Arabic + French
pdf-cli text scanned.pdf --ocr --ocr-lang eng+ara+fra
```

### Backend selection

```bash
# Auto (default): native if available, else wasm
pdf-cli text scanned.pdf --ocr

# Force native Tesseract (requires system tesseract)
pdf-cli text scanned.pdf --ocr --ocr-backend native

# Force WASM (built-in, no system dependency)
pdf-cli text scanned.pdf --ocr --ocr-backend wasm
```

---

## Real-World Tests with resume.pdf

The following tests were run against `~/pdfs/resume.pdf` (4 pages, 156.62 KB, PDF 1.5, not encrypted).

### info

```bash
$ pdf-cli info ~/pdfs/resume.pdf
File:       /data/data/com.termux/files/home/pdfs/resume.pdf
Size:       156.62 KB
Pages:      4
Version:    PDF 1.5
Encrypted:  false
Author:     Usama Khan
Creator:    Microsoft® Word 2016
Producer:   Microsoft® Word 2016
```

JSON output:

```bash
$ pdf-cli info ~/pdfs/resume.pdf --format json
{
  "file": "/data/data/com.termux/files/home/pdfs/resume.pdf",
  "size": 160378,
  "size_human": "156.62 KB",
  "pages": 4,
  "version": "1.5",
  "encrypted": false,
  "metadata": {
    "author": "Usama Khan",
    "creator": "Microsoft® Word 2016",
    "producer": "Microsoft® Word 2016"
  }
}
```

Multi-file summary:

```bash
$ pdf-cli info ~/pdfs/resume.pdf ~/pdf_cli_test_out/pages.pdf
FILE                                       PAGES    VER    SIZE
----------------------------------------------------------------------
resume.pdf                                     4    1.5  156.62 KB
pages.pdf                                      2    1.7  112.10 KB
```

### text

```bash
# Full text extraction
$ pdf-cli text ~/pdfs/resume.pdf
# Prints ~3 pages of formatted text to terminal

# Page-specific extraction
$ pdf-cli text ~/pdfs/resume.pdf -p 1 -o page1.txt

# JSON structured info for scripting
$ pdf-cli info ~/pdfs/resume.pdf --format json | jq '.pages'
4
```

### extract

```bash
# Extract pages 1 and 3
$ pdf-cli extract ~/pdfs/resume.pdf -p 1,3 -o pages.pdf
Extracted 2 pages to pages.pdf

$ pdf-cli info pages.pdf
Pages: 2, Version: PDF 1.7
```

### merge

```bash
# Merge two split files
$ pdf-cli merge split/resume_1.pdf split/resume_2.pdf -o merged.pdf
Merged 2 files into merged.pdf

$ pdf-cli info merged.pdf
Pages: 2, Size: 120.07 KB
```

### split

```bash
# Split into individual pages
$ pdf-cli split ~/pdfs/resume.pdf -o split/
Split into split/
$ ls split/
resume_1.pdf  resume_2.pdf  resume_3.pdf  resume_4.pdf

# Split into 2-page chunks
$ pdf-cli split ~/pdfs/resume.pdf -n 2 -o chunks/
$ ls chunks/
resume_1-2.pdf  resume_3-4.pdf
```

### rotate

```bash
$ pdf-cli rotate ~/pdfs/resume.pdf -a 90 -o rotated.pdf
Rotated all pages by 90 degrees

$ pdf-cli info rotated.pdf
Pages: 4, Size: 155.39 KB
```

### reorder

```bash
$ pdf-cli reorder ~/pdfs/resume.pdf -s "4,1,2,3" -o reordered.pdf
Reordered PDF saved to reordered.pdf (4 pages)
```

### compress

```bash
$ pdf-cli compress ~/pdfs/resume.pdf -o compressed.pdf
Compressed resume.pdf to compressed.pdf
Original:   156.62 KB
Compressed: 155.38 KB
Saved:      1.24 KB (0.8%)
```

> Note: This PDF was already well-optimized, so savings were minimal. More complex PDFs typically see larger reductions.

### encrypt / decrypt

```bash
# Encrypt
$ pdf-cli encrypt ~/pdfs/resume.pdf -o encrypted.pdf --password test123 --allow-insecure-password
WARNING: --password flag exposes passwords in process listings...
Encrypted to encrypted.pdf

# Verify encrypted
$ pdf-cli info encrypted.pdf
Error: password required

# Decrypt
$ pdf-cli decrypt encrypted.pdf -o decrypted.pdf --password test123 --allow-insecure-password
Decrypted to decrypted.pdf

$ pdf-cli info decrypted.pdf
Encrypted: false, Pages: 4
```

### images

```bash
$ pdf-cli images ~/pdfs/resume.pdf -o images/
Images extracted to images/
$ ls images/ | head -10
resume_1_X0.png
resume_1_X1.png
resume_1_X2.png
resume_1_X10.jpg
...
# 63 images total extracted
```

### meta

```bash
# View
$ pdf-cli meta ~/pdfs/resume.pdf
File: /path/to/resume.pdf
Author:     Usama Khan
Creator:    Microsoft® Word 2016
Producer:   Microsoft® Word 2016

# Update
$ pdf-cli meta ~/pdfs/resume.pdf --title "My Resume" --author "Usama Khan" -o meta_updated.pdf
$ pdf-cli meta meta_updated.pdf
Title:      My Resume
Author:     Usama Khan
```

### watermark

```bash
$ pdf-cli watermark ~/pdfs/resume.pdf -t "DRAFT" -o watermarked.pdf
Watermark added to watermarked.pdf
```

### pdfa

```bash
$ pdf-cli pdfa validate ~/pdfs/resume.pdf
✓ /path/to/resume.pdf passes basic PDF/A validation

Warnings:
  - Note: This is basic PDF/A validation. Full compliance testing requires veraPDF.

$ pdf-cli pdfa convert ~/pdfs/resume.pdf -o pdfa.pdf
PDF optimized and saved to pdfa.pdf
```

### Pipeline test

```bash
# Extract pages 1-2, then rotate — all via pipes
$ cat ~/pdfs/resume.pdf | pdf-cli extract - -p 1-2 --stdout | pdf-cli rotate - -a 90 --stdout > pipeline.pdf
$ pdf-cli info pipeline.pdf
Pages: 2, Size: 114.90 KB
```

### OCR test

```bash
# Plain text extraction (no OCR) - WORKS
$ pdf-cli text ~/pdfs/resume.pdf -o plain.txt
Extracted text saved to plain.txt
$ wc -l plain.txt
1873 plain.txt

# OCR extraction - POOR RESULT on text-type PDF
$ pdf-cli text ~/pdfs/resume.pdf --ocr --ocr-backend native -o ocr.txt
$ cat ocr.txt
qr
=
```

> **Finding:** `resume.pdf` is a **text-type PDF** (exported from Word), not a scan. Plain text extraction gets 1873 lines of readable content. OCR on text-type PDFs produces poor results (only "qr" and "=").
>
> **Rule of thumb:**
> - Text-type PDF (Word, digital) → Use `pdf-cli text doc.pdf` (no `--ocr`)
> - Scanned/image PDF → Use `pdf-cli text doc.pdf --ocr --ocr-backend native`

---

## Troubleshooting

| Problem | Solution |
|---|---|
| `command not found: pdf-cli` | Add `$(go env GOPATH)/bin` to PATH, or use the full path to the binary |
| `permission denied` on output | Ensure the output directory is writable; use `~` or a directory you own |
| `password required` on encrypted PDF | Use `--password`, `--password-file`, or set `PDF_CLI_PASSWORD` env var |
| No text extracted OR poor OCR result | Check if PDF is text-type (Word export) or scanned. For text-type, don't use `--ocr`. For scans, use `--ocr --ocr-backend native` |
| OCR returns garbage/short text | PDF is likely text-type, not scanned. Remove `--ocr` flag |
| Native Tesseract not detected | Install tesseract (`pkg install tesseract` on Termux), set `TESSDATA_PREFIX` |
| Large PDFs slow | Use `--progress`; parallel processing is automatic for >5 pages / >3 files |
| OCR fails with WASM | Install native Tesseract and use `--ocr-backend native` |
| CSV/TSV format errors | Only `json` is currently supported for `info` and `meta` output |
| `--password` security warning | Use `--password-file` or `PDF_CLI_PASSWORD` env var instead |

---

## Known Limitations

1. **CSV/TSV formats** — listed in help but not actually implemented for `info` and `meta`; only `json` works.
2. **PDF/A** — basic validation and optimization only; no full ISO 19005 compliance. Use veraPDF for validation and Ghostscript/Adobe Acrobat for conversion.
3. **OCR (WASM)** — the built-in WASM Tesseract backend may fail on some environments (e.g., Android/Termux) due to memory stack limitations. Use native Tesseract where available.
4. **merge --password** — the flag is listed but does not work for reading encrypted inputs.
5. **text --stdout** — this flag does not exist; text command outputs to stdout by default.
6. **Single binary** — no GUI; designed for terminal/script use.

---

## Quick Reference Card

```bash
# Inspect
pdf-cli info doc.pdf --format json
pdf-cli meta doc.pdf

# Extract text
pdf-cli text doc.pdf                    # Fast: for text-type PDFs (Word, digital)
pdf-cli text scanned.pdf --ocr          # Slow: for scanned/image PDFs only
pdf-cli text doc.pdf -p 1-5 -o text.txt

# Manipulate pages
pdf-cli extract doc.pdf -p 1,3,5-10 -o out.pdf
pdf-cli split doc.pdf -o out/
pdf-cli split doc.pdf -n 5 -o chunks/
pdf-cli rotate doc.pdf -a 90 -o out.pdf
pdf-cli reorder doc.pdf -s "end-1" -o reversed.pdf
pdf-cli compress doc.pdf -o smaller.pdf

# Combine
pdf-cli merge -o combined.pdf a.pdf b.pdf
pdf-cli combine-images *.png -o album.pdf

# Security
pdf-cli encrypt doc.pdf --password-file pass.txt -o secure.pdf
pdf-cli decrypt secure.pdf --password-file pass.txt -o open.pdf

# Watermark
pdf-cli watermark doc.pdf -t "CONFIDENTIAL" -o marked.pdf

# Pipeline
cat in.pdf | pdf-cli extract - -p 1-3 --stdout | pdf-cli rotate - -a 90 --stdout > out.pdf
```
