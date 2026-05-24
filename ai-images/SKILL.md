---
name: ai-images
description: Generate and edit images using FLUX.2-klein, Z-Image-Turbo, Qwen-Image, Qwen3-VL, Qwen3.5-9B, and GLM-OCR models on Apple Silicon. Use when user wants to create, modify, or generate images, describe images, or extract text from images/PDFs.
---

# AI Image Tools

This skill provides CLI tools for AI image generation and editing on Apple Silicon (MPS).

## Available Tools

- **`igm.flux-gen`** - Text-to-image using FLUX.2-klein-9B (4 steps, fast)
- **`igm.flux-edit`** - Image-to-image editing using FLUX.2-klein-9B
- **`igm.z-gen`** - Text-to-image using Z-Image-Turbo (9 steps)
- **`igm.qwen-gen`** - Text-to-image using Qwen-Image-2512 (supports turbo/lightning modes)
- **`igm.qwen-desc`** - Image-to-text description using Qwen3-VL-8B-Instruct
- **`igm.qwen35-desc`** - Image-to-text description using Qwen3.5-9B (with thinking mode)
- **`igm.glm-ocr`** - Image/PDF-to-text OCR using GLM-OCR (supports text, formula, table)

## When to Use

Use these tools when the user wants to:
- Generate an image from a text description
- Edit or transform an existing image
- Create variations of an image
- Apply specific styles to images
- Describe or analyze an image
- High-quality image description with reasoning
- Extract text from images or PDFs (OCR)
- Extract mathematical formulas from images
- Extract table data from images

## Tool Selection

| Use case | Recommended tool |
|----------|------------------|
| Fast text-to-image | `igm.flux-gen` (4 steps) |
| Image editing/transforms | `igm.flux-edit` |
| Alternative text-to-image | `igm.z-gen` |
| High quality / aspect ratios | `igm.qwen-gen` |
| Very fast generation | `igm.qwen-gen --turbo` or `--lightning` |
| Image description/analysis | `igm.qwen-desc` |
| High-quality image description | `igm.qwen35-desc` |
| Complex visual reasoning | `igm.qwen35-desc --think` |
| Extract text from images/PDFs | `igm.glm-ocr` |
| Extract formulas from images | `igm.glm-ocr -p formula` |
| Extract tables from images | `igm.glm-ocr -p table` |

## Usage Patterns

### Basic Generation

```bash
igm.flux-gen "a serene mountain landscape at sunset"
igm.flux-gen "portrait of a warrior" -w 768 -h 1024
```

### Image Editing

```bash
igm.flux-edit "turn into oil painting" -i photo.jpg
igm.flux-edit "anime style" -i portrait.jpg -o anime.png
```

### Fast Generation (Qwen)

```bash
igm.qwen-gen "a cat sitting on a fence" --turbo
igm.qwen-gen "cyberpunk city" --lightning
```

### Custom Resolution

```bash
igm.flux-gen "landscape" -w 1920 -h 1080
igm.qwen-gen "portrait" -w 1472 -h 1104  # 4:3 aspect ratio
```

### Reproducible Results

```bash
igm.flux-gen "a cat" --seed 42
```

### Image Description

```bash
igm.qwen-desc photo.jpg
igm.qwen-desc portrait.jpg -p "Describe the person's expression"
igm.qwen-desc https://example.com/image.jpg -q
```

### High-Quality Image Description (Qwen3.5)

```bash
igm.qwen35-desc photo.jpg
igm.qwen35-desc diagram.jpg -p "Explain this diagram in detail"
igm.qwen35-desc puzzle.jpg --think  # Enable thinking mode for complex analysis
igm.qwen35-desc https://example.com/image.jpg -q
```

### OCR - Extract Text from Images/PDFs

```bash
# Extract text from image
igm.glm-ocr document.jpg

# Extract mathematical formulas (LaTeX)
igm.glm-ocr formula.jpg -p formula

# Extract table data
igm.glm-ocr table.jpg -p table

# Process entire PDF
igm.glm-ocr document.pdf

# Process specific PDF page
igm.glm-ocr document.pdf --page 3

# From URL
igm.glm-ocr https://example.com/document.png
```

## Common Options

All tools support:
- `-o, --output` - Output filename
- `-w, --width` - Image width
- `-h, --height` - Image height
- `-s, --steps` - Inference steps
- `--seed` - Random seed
- `-q, --quiet` - Less verbose output
- `--help` - Show help

## Notes

- All tools require Apple Silicon (MPS)
- Images are saved with metadata stripped
- Output files auto-increment (_1, _2) if file exists
- Memory optimizations enabled by default
- PDF support for OCR requires poppler (`brew install poppler`)
