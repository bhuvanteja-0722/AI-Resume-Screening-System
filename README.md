# TalentLens — AI Resume Screening

A recruiter dashboard with a working local demo flow and optional Groq AI integration. Start from a role prompt, create a job description, then upload PDF, DOCX, or TXT resumes. Without a provider key the server uses a clearly labeled keyword overlap estimate. Groq is preferred when configured; NVIDIA remains available as a fallback provider.

## Run locally

1. Use Python 3.10 or newer.
2. Create a virtual environment and install dependencies:

   ```sh
   python -m venv .venv
   source .venv/bin/activate  # Windows: .venv\Scripts\activate
   pip install -r requirements.txt
   ```

3. For AI analysis, copy `.env.example` to `.env` and set `GROQ_API_KEY` to your key from [Groq Console](https://console.groq.com/keys). The default model is `openai/gpt-oss-20b`. Never paste the key into chat or frontend code.
4. Start the app (it works without a key in local demo mode):

   ```sh
   uvicorn server:app --reload
   ```

5. Open http://127.0.0.1:8000.

The Groq credential is used only by the local server and is never sent to the browser. When Groq is enabled, extracted resume text is sent to Groq's chat-completions endpoint. The default model is `openai/gpt-oss-20b`; change `GROQ_MODEL` in `.env` to select another available model. If Groq times out or is temporarily unavailable, the app returns a clearly labeled local keyword estimate instead of a server error. NVIDIA remains as a fallback if `GROQ_API_KEY` is unset. Provider availability, quotas, data handling, and terms can change; review the provider's current documentation before using real resumes.

The dashboard starts with sample candidate rows for preview. Uploading resumes replaces those rows with this screening's results. Local keyword scores are not semantic and can miss equivalent terms; AI results can also be wrong. Do not use a match score as a hiring decision: review source resumes and make decisions with human oversight.

## Deploy to Render

1. Push this repository to GitHub.
2. In Render, choose **New → Blueprint**, then connect the repository. Render reads `render.yaml` and prepares a free web service.
3. In Render's Environment settings, set `GROQ_API_KEY` to your Groq secret. Do not add the key to GitHub. The service also works in local keyword mode if you leave provider keys unset.
4. Deploy and open the generated `onrender.com` URL. Render checks `/api/status` to confirm the service is up.

The free web service may spin down after inactivity, so its first request after a quiet period can take longer. This app keeps uploaded resume files in memory for the request and does not save candidate records between sessions.
