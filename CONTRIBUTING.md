# Contributing to mu-pdf-converter

Thank you for your interest in contributing! Here's how you can help.

## Getting Started

1. Fork the repository
2. Clone your fork: `git clone https://github.com/YOUR_USERNAME/mu-pdf-converter.git`
3. Install dependencies: `pip install -r requirements.txt`
4. Create a feature branch: `git checkout -b feature/your-feature`

## Development Setup

```bash
# Install all dependencies including optional ones
pip install -e ".[all]"

# Run a quick test
python scripts/pdf_to_pptx.py sample.pdf --outfile test.pptx
```

## Submitting Changes

1. Commit your changes with clear commit messages
2. Push to your fork
3. Open a Pull Request with a description of what you changed and why

## Code Style

- Follow PEP 8
- Add docstrings to new functions
- Keep functions focused and small
- Add error handling with helpful messages

## Reporting Issues

- Use GitHub Issues
- Include: Python version, OS, input PDF characteristics, full error traceback
- If possible, provide a sample PDF that reproduces the issue

## Areas We'd Love Help With

- Improving borderless table detection accuracy
- Adding support for more languages in translation
- Performance optimization for large PDFs
- Better handling of complex layouts (multi-column, mixed text/image)
