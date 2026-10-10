# SKRIPTA

Syllabus topics → handwritten-style revision notes with flowcharts. Static page (`index.html`) + one serverless function (`api/generate.js`) that calls OpenRouter. The API key stays on the server.

## 1. Get a free key (Groq)
1. Sign in at https://console.groq.com, open API Keys (console.groq.com/keys) and create a key (pick a long expiry, e.g. 90 days). Copy it once.
2. Free limits are per model and per organization; check your account's limits page. Suggested model: `openai/gpt-oss-120b`.
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
2. Project Settings → Environment Variables: add `LLM_API_KEY`, `LLM_BASE_URL` and `LLM_MODEL`.
3. Redeploy, open the URL, generate 1 topic, then 10.

## Local run (optional)
    npm i -g vercel && vercel dev   # with the two variables in .env.local

## Notes
- The rate limit in `api/generate.js` is in-memory and resets on cold starts. It is not production-grade; use Upstash for that.
- Never commit `.env*` and never put the key in `index.html`. If it leaks, delete it on your provider's keys page and create a new one.
- Android: see docs/ANDROID.md (PWA files are included).
- Printing uses the browser's "Save as PDF". No text editing, topic reordering or page-fit engine yet.
