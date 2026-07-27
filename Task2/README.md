# DevSynt AI Automation Internship — [Your Name]

**Track:** AI Automation
**Mentor:** Afnan Shoukat
**Summer 2026**

This repo contains my weekly task progress for the DevSynt AI Automation Internship.

---

## Task 1 — Set Up n8n and Push Progress to GitHub

n8n and ngrok are run via Docker Compose instead of local `npm install`, so the whole setup is
reproducible on any machine with one command.

**Files:** `docker/docker-compose.yml`, `docker/.env.example`

**How to run it:**
```bash
cd docker
cp .env.example .env
# edit .env: add your NGROK_AUTHTOKEN and NGROK_DOMAIN (claimed from dashboard.ngrok.com/domains)
docker compose up -d
```
- n8n editor: `http://localhost:5678` (local) or `https://<your-static-domain>.ngrok-free.app` (public)
- ngrok inspector: `http://localhost:4040`

Both containers restart automatically (`restart: unless-stopped`), so they survive reboots as long
as Docker is running.

**Status:** ✅ n8n reachable via static ngrok domain — see submission screenshot on the portal.

---

## Task 2 — WhatsApp Automation, Phase 1 (Design + Sandbox)

**Niche chosen:** Dental clinic ("Bright Smile Dental Clinic")

**Folder:** `task2-whatsapp-phase1/`
```
task2-whatsapp-phase1/
├── assets/
│   ├── flow-diagram.mermaid       ← full conversation state diagram
│   └── webhook-test-screenshot.png ← add your own screenshot here
├── workflow.json                  ← export from n8n: Workflow → Download
├── messages.md                    ← all 9 messages × EN/AR (18 versions)
└── config.json                    ← reusable per-client config (bonus)
```

### What's designed
- **State 0–5** conversation flow (language detection → greeting/intent → service → timing →
  slot offer → confirmation), diagrammed in `assets/flow-diagram.mermaid`.
- **Bilingual (EN/AR)** messages for every state, all 3 no-reply nudges, and the human handoff —
  written out in `messages.md`.
- **No-reply nudges** at +1h (free-form), +24h and +72h (flagged as needing Meta-approved template
  messages in production, since WhatsApp only allows free-form replies within a 24h window).
- **Human handoff** rules: medical/health questions, complaints, pricing negotiation, or anything
  off-script escalates to a human instead of the bot guessing.
- **Reusable config** (`config.json`): clinic name, services, calendar ID, handoff contact, and
  reminder timing are all externalized so Phase 2 (new client) is a config edit, not a rebuild.

### Sandbox → n8n connection
- WhatsApp sandbox set up via Meta Cloud API (free developer tier).
- n8n Webhook node created; its local URL's host was swapped for the Task 1 static ngrok domain.
- That full webhook URL was pasted into Meta's App Dashboard → WhatsApp → Configuration →
  Webhook, with a self-chosen Verify Token, and the `messages` field subscribed.
- Verified: a message sent from the sandbox test number shows up as a successful execution in n8n
  (see `assets/webhook-test-screenshot.png`).

**Security note:** No Meta access token or verify token is committed to this repo. Add your own in
n8n's credential store (Workflow → Credentials), never in code or `.env` files that get pushed.

**What's NOT built yet (by design — that's Phase 2):** real calendar integration, actual message
routing logic in n8n, and the AI/NLP layer for free-text understanding.

---

## GitHub Work

This section documents how the repo itself is managed, separately from the automation content.

**Repo name:** `devsynt-ai-internship-[yourname]`

**Commit strategy:** one commit per completed task/chunk — not one big commit at the end.

**Setup commands used:**
```bash
# one-time repo init
git init
git branch -M main
git remote add origin https://github.com/<yourusername>/devsynt-ai-internship-[yourname].git

# Task 1 commit
git add README.md docker/
git commit -m "Task 1: n8n + ngrok Docker setup with static domain"
git push -u origin main

# Task 2 commit
git add task2-whatsapp-phase1/ README.md
git commit -m "Task 2: WhatsApp bot Phase 1 - flow design, bilingual messages, webhook sandbox"
git push
```

**`.gitignore` (create this at repo root — do not skip it):**
```
docker/.env
node_modules/
.n8n/
*.log
```

**Rule of thumb going forward:** every new task = its own folder (`task3-.../`, `task4-.../`) and its
own commit(s). Never dump multiple tasks into a single commit, and never commit real credentials
(`.env`, access tokens, verify tokens) — only the `.env.example` template.
