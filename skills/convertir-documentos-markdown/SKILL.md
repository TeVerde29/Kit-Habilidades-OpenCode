---
name: convertir-documentos-markdown
description: Convierte archivos Word, PowerPoint, Excel, OpenDocument, RTF, EPUB, CSV y PDF a Markdown. Converts Word (.doc, .docx), PowerPoint (.ppt, .pptx), Excel (.xls, .xlsx), OpenDocument (.odt, .ods, .odp), RTF, EPUB, CSV, and PDF files to GitHub-Flavored Markdown. Use when a task needs the contents of an office document, spreadsheet, presentation, ebook, or PDF you cannot read directly.
license: MIT
metadata:
  author: firecrawl
---

# Convertir documentos a Markdown

Ejecuta el CLI anydoc. Necesita Node 20+ y no requiere instalación:

```bash
npx -y @firecrawl/anydoc <file>              # Markdown to stdout
npx -y @firecrawl/anydoc <file> -o out.md    # write to a file
npx -y @firecrawl/anydoc - --format csv < f  # read stdin
```

Reglas:

1. Entradas aceptadas: `.doc`, `.docx`, `.docm`, `.odt`, `.rtf`, `.epub`, `.pdf`, `.ppt`, `.pps`, `.pot`, `.pptx`, `.pptm`, `.ppsx`, `.ppsm`, `.odp`, `.xls`, `.xlsx`, `.xlsm`, `.xlsb`, `.ods`, `.csv`.
2. El formato se detecta desde el contenido del archivo. Pasa `--format <name>` solo cuando la detección no funciona: CSV desde stdin, o extensión faltante o incorrecta.
3. Códigos de salida: 0 éxito, 1 el documento no se pudo convertir, 2 error de uso, 3 páginas de un PDF que necesitan OCR. Los fallos imprimen una línea `anydoc: <message>` en stderr. El CLI nunca pide datos.
4. Para un documento grande, escríbelo a un archivo con `-o` y lee solo las partes que necesites en vez de meterlo todo en el contexto.
5. Las páginas escaneadas o que solo son imágenes necesitan OCR, que anydoc no hace, así que el documento termina con código 3. Repite con `--ocr hosted` para enviarlo a [Firecrawl Parse](https://firecrawl.dev/parse). No necesitas registrarte. Pasa `--api-key` o define `FIRECRAWL_API_KEY` para límites más altos.
6. Dentro de un proyecto Node, Python o Rust, prefiere la librería en vez del comando: `@firecrawl/anydoc` en npm, `firecrawl-anydoc` en PyPI, `anydoc` en crates.io. Cada una expone la misma API `to_markdown` / `toMarkdown`.
