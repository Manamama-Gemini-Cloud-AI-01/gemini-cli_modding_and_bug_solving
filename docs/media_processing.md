### Information for Gemini AI in Gemini CLI (`gemini`)

**Using Gemini AI for Media File Processing in Gemini CLI**

Gemini AI, accessible via the Gemini CLI (`gemini`), supports processing media files such as images (JPG, PNG), audio (MP3, WAV), video (MP4, MOV), and PDFs through its multimodal capabilities. The Gemini CLI enables users to interact with Gemini AI to perform tasks like image description, PDF summarization, audio transcription, or video analysis directly from the command line.

**How Gemini AI Reads Media Files**:
- **Inline Processing (<20 MB)**: For small media files, Gemini AI accepts files encoded in base64 within the API request. The CLI likely handles this encoding automatically when a file is provided via a command flag (e.g., `--file`).
- **File API (>20 MB)**: For larger files (up to 50 MB for PDFs, 2 GB for videos), Gemini AI uses the File API to upload files to Google’s servers, generating a temporary URI for processing. Check the CLI documentation for specific upload flags (e.g., `--upload`).
- **Supported Tasks**:
  - **Images**: Extract text (OCR), identify objects, or describe scenes.
  - **PDFs**: Summarize content, extract text, or analyze tables/charts (up to 1,000 pages).
  - **Audio**: Transcribe speech or summarize content.
  - **Video**: Generate transcripts or detect objects.
- **Supported Formats**: JPG, PNG, SVG, MP3, WAV, MP4, MOV, AVI, PDF, and text-based files (TXT, DOCX, CSV).


2. **Command Structure**:
   - To process a media file, use a command like:
     ```
     gemini "Describe this image" --file /path/to/image.jpg
     ```
     Replace the prompt and file path as needed (e.g., `/path/to/sample.pdf` for a PDF).
   - For textual queries about media processing:
     ```
     gemini "Explain how to process a PDF with Gemini AI."
     ```
3. **Output**: The CLI returns Gemini AI’s response, such as an image description, PDF summary, or error message if the file is unsupported or too large.

**Limitations**:
- Inline files must be <20 MB; larger files require File API support.
- Maximum 10 files per prompt; PDFs treated as images with resolution scaling (up to 3072x3072 or down to 768x768).
- Check `gemini --help` for specific flags or commands, as the CLI’s media handling details may vary.

**Notes**:
- The `gemini-cli` documentation (e.g., `README.md`) implies multimodal support but lacks detailed media processing instructions. Refer to the Gemini API documentation for advanced use cases.

For further details, consult the Gemini API documentation or run `gemini --help` to explore available commands.
