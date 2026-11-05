# TalentLens — AI Resume Screening

A recruiter dashboard with a working local demo flow and optional NVIDIA NIM integration. Start from a role prompt, create a starter job description, then upload PDF, DOCX, or TXT resumes. Without an NVIDIA key the server uses a clearly labeled keyword overlap estimate; with a key it uses NVIDIA's OpenAI-compatible API for candidate summaries and match estimates.

## Run locally

1. Use Python 3.10 or newer.
2. Create a virtual environment and install dependencies:

   ```sh
   python -m venv .venv
   source .venv/bin/activate  # Windows: .venv\Scripts\activate
   pip install -r requirements.txt
   ```

3. NVIDIA is optional. For live NVIDIA analysis, copy `.env.example` to `.env` and set `NVIDIA_API_KEY` to your key from [NVIDIA Build](https://build.nvidia.com/). Never paste the key into chat or frontend code.
4. Start the app (it works without a key in local demo mode):

   ```sh
   uvicorn server:app --reload
   ```

5. Open http://127.0.0.1:8000.

The NVIDIA credential is used only by the local server and is never sent to the browser. When NVIDIA is enabled, extracted resume text is sent to the configured NVIDIA endpoint. The default model is `openai/gpt-oss-20b`; change `NVIDIA_MODEL` in `.env` to select another available endpoint. NVIDIA endpoint availability, quotas, and terms can change; check the model page.

The dashboard starts with sample candidate rows for preview. Uploading resumes replaces those rows with this screening's results. Local keyword scores are not semantic and can miss equivalent terms; NVIDIA results can also be wrong. Do not use a match score as a hiring decision: review source resumes and make decisions with human oversight.

## Deploy to Render

1. Push this repository to GitHub.
2. In Render, choose **New → Blueprint**, then connect the repository. Render reads `render.yaml` and prepares a free web service.
3. During Blueprint setup, enter your NVIDIA key for `NVIDIA_API_KEY` in Render's secure field. Do not add the key to GitHub. The service also works in local keyword mode if you leave the key unset.
4. Deploy and open the generated `onrender.com` URL. Render checks `/api/status` to confirm the service is up.

The free web service may spin down after inactivity, so its first request after a quiet period can take longer. This app keeps uploaded resume files in memory for the request and does not save candidate records between sessions.
