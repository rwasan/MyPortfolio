# Rahul Wasan — Portfolio (GitHub Pages)

This repository contains a single-page, responsive portfolio for **Rahul Wasan**.
Included is a chat widget that you can connect to an n8n webhook. The chat now supports:
- Message timestamps
- Persistent `sessionId` (stored in `localStorage`) so your n8n workflow can maintain context
- Client-side conversation persistence (in `sessionStorage`)
- Typing indicator and streaming-like UI (client-side simulated streaming)

## Files included
- `index.html` — main site file (self-contained)
- `README.md` — this file

## How to deploy
1. Create a GitHub repository and push these files to the `main` branch.
2. Enable GitHub Pages (Settings → Pages) from the `main` branch (root).
3. Visit `https://<your-username>.github.io/<repo>/` after a minute.

## n8n webhook integration
Replace the placeholder webhook URL in `index.html`:
```js
const WEBHOOK_URL = 'https://your-n8n.example.com/webhook/rahul-chat';
```

### Expected webhook behavior
- The site sends `POST` with JSON: `{ "message": "<text>", "sessionId": "<uuid>", "source":"portfolio-website" }`
- Your n8n workflow should return JSON like: `{ "reply": "Hello — got your message" }`

### Example minimal n8n workflow
1. **Webhook** node — POST on `/webhook/rahul-chat`.
2. **Set** node — set `reply` to a string using the incoming payload, e.g.:
   - `{{$json["body"]["message"]}}` or `"Hello, I got: {{$json["body"]["message"]}}"`
3. **Respond to Webhook** — return JSON body: `{ "reply": <value> }`

Make sure CORS is allowed (add header `Access-Control-Allow-Origin: *` if needed) so browser requests succeed.

## Notes & next steps
- For true server-side streaming you'd need a backend that sends chunked responses (e.g., SSE or HTTP chunking). The current UI simulates streaming on the client for an improved visual experience.
- If you'd like, I can also:
  - Create a ZIP file of this project (I already did — download link below).
  - Add a GitHub Action to auto-deploy to `gh-pages` branch.
  - Add a small `assets/` folder with icons and a `CNAME` file for a custom domain.
