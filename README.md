# SKRIPTA

Syllabus topics → handwritten-style revision notes with flowcharts. Static page (`index.html`) + one serverless function (`api/generate.js`) that calls OpenRouter. The API key stays on the server.

## 1. Get a free key (Groq)
1. Sign in at https://console.groq.com, open API Keys (console.groq.com/keys) and create a key (pick a long expiry, e.g. 90 days). Copy it once.
2. In Vercel Project Settings → Environment Variables, set all three values: `LLM_API_KEY` to the Groq key, `LLM_BASE_URL` to `https://api.groq.com/openai/v1`, and `LLM_MODEL` to `openai/gpt-oss-120b`.
3. Add them for the environment you deploy to, then redeploy so the serverless function receives them. Groq limits are per model and per organization; check your account's limits page if requests are rejected.
Any OpenAI-compatible provider works by changing the three `LLM_*` variables (OpenRouter: base `https://openrouter.ai/api/v1`, model slug ending `:free`).

## 2. Run the tests (no key needed, OpenRouter is mocked)
    npm test

## 3. Push to GitHub
    git init && git add . && git commit -m "SKRIPTA"
    git branch -M main
    git remote add origin https://github.com/<you>/skripta.git
    git push -u origin main

## 4. Deploy on Vercel
1. Import the GitHub repo at https://vercel.com/new (no build command, no framework).
2. Project Settings → Environment Variables: add `LLM_API_KEY`, `LLM_BASE_URL` and `LLM_MODEL` (for Groq, use the exact values above).
3. Deploy, open the URL, generate 1 topic, then 10. Errors now appear on the generation screen; if you changed variables, confirm the deployment is the new one.

To update an existing deployment, push the changes to its connected Git branch or use **Redeploy** on the latest deployment in the Vercel dashboard. This replaces the running version without deleting the project, URL, or environment variables. Delete the Vercel project only if you intentionally want to remove the app and its configuration.

## Local run (optional)
1. Install the Vercel CLI: `npm i -g vercel`.
2. Copy `.env.example` to `.env.local` and set `LLM_API_KEY` to your Groq key. The base URL and model are prefilled for Groq; `.env.local` is ignored by Git.
3. In PowerShell, load the ignored settings into the current process, then start Vercel:
   ```powershell
   Get-Content .env.local | ForEach-Object {
       if ($_ -match '^\s*([A-Za-z_][A-Za-z0-9_]*)\s*=\s*(.*)\s*$') {
           [Environment]::SetEnvironmentVariable($Matches[1], $Matches[2].Trim().Trim('"').Trim("'"), 'Process')
       }
   }
   vercel dev
   ```
4. Open the local URL it prints. Stop any existing `vercel dev` process first if the default port is already in use.

## Notes
- The rate limit in `api/generate.js` is in-memory and resets on cold starts. It is not production-grade; use Upstash for that.
- Never commit `.env*` and never put the key in `index.html`. If it leaks, delete it on your provider's keys page and create a new one.
- Android: see docs/ANDROID.md (PWA files are included).
- "Download PDF" downloads the rendered note pages as an A4 PDF; "Print / Save as PDF" opens the browser print dialog as an alternative.
- Notes default to one page per topic. Users can choose up to five pages per topic in the generation preferences.
- No text editing, topic reordering or page-fit engine yet.
