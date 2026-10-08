# SKRIPTA

Syllabus topics → handwritten-style revision notes with flowcharts. Static page (`index.html`) + one serverless function (`api/generate.js`) that calls OpenRouter. The API key stays on the server.

## 1. Get an OpenRouter key and a free model
1. Sign up at https://openrouter.ai, then create a key at https://openrouter.ai/keys. Set a credit limit on it.
2. Open https://openrouter.ai/models, filter for **free** models, and copy a slug that ends in `:free`.
   Free models are rate-limited and vary in quality; if notes come back malformed, try another.

## 2. Run the tests (no key needed, OpenRouter is mocked)
    npm test

## 3. Push to GitHub
    git init && git add . && git commit -m "SKRIPTA"
    git branch -M main
    git remote add origin https://github.com/<you>/skripta.git
    git push -u origin main

## 4. Deploy on Vercel
1. Import the GitHub repo at https://vercel.com/new (no build command, no framework).
2. Project Settings → Environment Variables: add `OPENROUTER_API_KEY` and `OPENROUTER_MODEL`.
3. Redeploy, open the URL, generate 1 topic, then 10.

## Local run (optional)
    npm i -g vercel && vercel dev   # with the two variables in .env.local

## Notes
- The rate limit in `api/generate.js` is in-memory and resets on cold starts. It is not production-grade; use Upstash for that.
- Never commit `.env*` and never put the key in `index.html`. If it leaks, delete it on openrouter.ai/keys and create a new one.
- Android: see docs/ANDROID.md (PWA files are included).
- Printing uses the browser's "Save as PDF". No text editing, topic reordering or page-fit engine yet.
