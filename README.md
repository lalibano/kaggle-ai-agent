# KaggleOS - AI Agent with Terminal & Workspaces

Fixed website: https://amaankhan231.github.io/kaggle-ai-agent/

## Features
- 🤖 Autonomous + Chat + Based + Assistant
- 💻 Terminal access (install OK, delete needs confirm)
- 📁 Workspaces per chat, persists to GitHub
- ⚡ Parallel background tasks
- 🔔 Notifications via ntfy.sh + Gmail
- 🧠 Multi-model via OpenRouter (Claude, GPT-4o, Llama)

## How it works
1. Kaggle notebook `kaggle_ai_agent.ipynb` runs FastAPI backend on :8000
2. Cloudflare Tunnel exposes it publicly (e.g., https://xxx.trycloudflare.com)
3. This URL is auto-written to `config.json` via GitHub API
4. You open fixed GitHub Pages URL, it reads config.json and connects to live Kaggle agent

## Setup

### Kaggle
1. Create new Kaggle notebook
2. Add Secrets: `OPENROUTER_API_KEY`, `NTFY_TOPIC`
3. Enable Internet in settings
4. Copy `kaggle_ai_agent/kaggle_ai_agent.ipynb` content and run
5. Get public URL from `/kaggle/working/tunnel_url.txt`

### GitHub Pages
1. Create repo `kaggle-ai-agent`
2. Push this folder to `main` branch
3. Enable GitHub Pages: Settings → Pages → Source: GitHub Actions
4. Your fixed URL: `https://<username>.github.io/kaggle-ai-agent/`

### ntfy.sh (No account needed)
1. Install ntfy app on phone (Android/iOS)
2. Subscribe to topic: `kaggle-agent-amaankhan231` (or your custom)
3. Add topic to Kaggle Secrets as `NTFY_TOPIC`
4. Agent will send push notifications automatically

### Gmail (via Composio)
- Connect Gmail via Composio dashboard
- Agent will send email notifications for important tasks

## Workspaces
Each chat has dedicated workspace in `/kaggle/working/workspaces/chat_xxx/files/`
- Isolated per chat
- Persists to GitHub via `workspaces/` folder in this repo
- Auto-synced every time Kaggle kernel pushes

## Permissions
- `pip install`, `npm install`, file write: No confirmation needed
- `rm`, `delete`, `rm -rf /`: Needs `confirm=True` in UI

## Architecture
```
[Fixed Website - GitHub Pages]
        |
        +---> [GitHub Actions] - Reminders, persistence, deploy
        |
        +---> [Kaggle Kernel] - Heavy compute, terminal, files
                - FastAPI :8000
                - Cloudflare Tunnel → public URL
                - OpenRouter multi-model
```

## Local Dev
Open `index.html` directly or run:
```bash
python -m http.server 8000
```
Then open http://localhost:8000

## Security
- OpenRouter key stored in Kaggle Secrets, never in code
- ntfy topic is random, acts as password
- Delete operations need confirmation
