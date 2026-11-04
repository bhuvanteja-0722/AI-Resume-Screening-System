# TalentLens — AI Resume Screening

A recruiter dashboard with NVIDIA NIM powered job description drafting and resume screening. Start from a role prompt, draft a job description, then upload PDF, DOCX, or TXT resumes; the server extracts the text and requests evidence-based candidate summaries and match scores from NVIDIA's OpenAI-compatible API.

## Run locally

1. Use Python 3.10 or newer.
2. Create a virtual environment and install dependencies:

   ```sh
   python -m venv .venv
   source .venv/bin/activate  # Windows: .venv\Scripts\activate
   pip install -r requirements.txt
   ```

3. Copy `.env.example` to `.env` and set `NVIDIA_API_KEY` to your key from [NVIDIA Build](https://build.nvidia.com/).
4. Start the app:

   ```sh
   uvicorn server:app --reload
   ```

5. Open http://127.0.0.1:8000.

The NVIDIA credential is used only by the local server and is never sent to the browser. The default model is `openai/gpt-oss-20b`; change `NVIDIA_MODEL` in `.env` to select another available NVIDIA endpoint. NVIDIA's free serverless endpoint availability, quotas, and terms can change; check the model page.

The dashboard includes sample candidate rows for preview. Uploading resumes replaces the top of that list with live analysis results. Do not use a match score as a hiring decision: review source resumes and make decisions with human oversight.
