# OCR Reader

A single-file Arabic OCR and translation tool built with plain HTML, CSS, and JavaScript.

## Features

- Upload images for OCR: PNG, JPG, WEBP, BMP, and TIFF.
- Upload PDF files and read the first page after converting it to an image.
- Choose OCR language: Arabic, English, Arabic + English, or French.
- Translate extracted text to Arabic, English, or French.
- Copy extracted or translated text.
- Works as a static browser page.

## Files

- `ocr.html` - The complete OCR app.
- `README.md` - This documentation file.

## How To Run

From this folder:

```powershell
python -m http.server 8000 --bind 127.0.0.1
```

Then open:

```text
http://127.0.0.1:8000/ocr.html
```

You can also open `ocr.html` directly in a browser, but using a local server is more reliable for browser APIs and external libraries.

## Requirements

- A modern browser.
- Internet access for CDN libraries and translation.
- Python is only needed if you want to run the local server command above.

## External Libraries

The page loads these libraries from CDN:

- Tesseract.js for OCR.
- PDF.js for rendering the first page of PDF files.
- Google Fonts for the Cairo and JetBrains Mono fonts.

## OCR Engine

This app uses **Tesseract.js v5.0.4** for OCR.

It is loaded in `ocr.html` from CDN:

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/tesseract.js/5.0.4/tesseract.min.js"></script>
```

The selected language is passed to Tesseract when creating the OCR worker, then the uploaded image is recognized:

```js
state.worker = await Tesseract.createWorker(lang, 1, { ... });
const { data: { text } } = await worker.recognize(source);
```

For PDF files, the app first uses PDF.js to render the first page into a canvas image. That canvas is then sent to Tesseract.js for OCR.

Translation uses Google Translate's public web endpoint. It does not require an API key, but it depends on network availability and may be rate-limited.

## Notes

- PDF OCR currently reads the first page only.
- OCR accuracy depends on image quality, text clarity, and selected language.
- For best results, use high-resolution scans with good contrast.
