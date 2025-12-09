AI-Powered Document Orchestrator (OpenAI backend)

A minimal, polished Streamlit app that extracts text from uploaded documents (PDF/DOCX/TXT/Image), analyzes them with OpenAI, and sends structured results to an n8n webhook for approval automation.

## Files

* `app.py` — main Streamlit app
* `extractor.py` — text extraction helpers
* `openai_client.py` — OpenAI API wrapper
* `utils.py` — file saving helpers
* `requirements.txt` — Python dependencies
* `.streamlit/secrets.toml` — (not in repo) store your secrets here
* `n8n_workflow.json` — example n8n workflow (template)

## Setup (local)

1. Create a Python venv and activate it:

   ```bash
   python -m venv venv
   venv\Scripts\activate   # Windows
   source venv/bin/activate  # macOS / Linux
   ```

2. Install dependencies:

   ```bash
   pip install -r requirements.txt
   ```

3. Create `.streamlit/secrets.toml` in the project root:

   ```toml
   OPENAI_API_KEY = "sk-..."
   N8N_WEBHOOK_URL = "https://your-n8n-host/webhook/..."
   ```

4. Run the app:

   ```bash
   streamlit run app.py
   ```

## n8n workflow (concept)

* Webhook node receives POST from app
* If node checks `analysis.confidence` (if your prompt returns confidence) or other rules
* Email node (SMTP/Gmail) sends approval or reviewer email
* Optional: archive JSON to Google Drive or S3

## Deployment (Streamlit Cloud)

* Push repository to GitHub
* In Streamlit Cloud, add `OPENAI_API_KEY` and `N8N_WEBHOOK_URL` in app secrets
* Set entrypoint to `app.py` and deploy

## Notes

* Tesseract OCR: if you expect scanned PDFs or images, install Tesseract engine for your OS. On Windows, install Tesseract and ensure `tesseract.exe` is in PATH.
* OpenAI model availability depends on your account; change `PRIMARY_MODEL` in `openai_client.py` if you use a different model.
