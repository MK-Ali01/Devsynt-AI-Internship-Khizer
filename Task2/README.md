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

```
- n8n editor: `http://localhost:5678` (local) or `https://<your-static-domain>.ngrok-free.app` (public)
- ngrok inspector: `http://localhost:4040`

Both containers restart automatically (`restart: unless-stopped`), so they survive reboots as long
as Docker is running.

**Status:** ✅ n8n reachable via static ngrok domain — see submission screenshot on the portal.

**Note on a third container (`cloudflared`):** added in Task 2, not Task 1. ngrok's free tier shows
an interstitial warning page (`ERR_NGROK_6024`) on every request, which a human can click through in
a browser but which silently blocks automated server-to-server calls like Meta's webhook
verification. So: **ngrok** stays as the Task 1 deliverable and for my own browser access to the n8n
editor; **cloudflared** (no interstitial) is what Meta's webhook actually talks to. See Task 2 below.

---

## Task 2 — WhatsApp Automation, Phase 1 (Design + Sandbox)

**Niche chosen:** Dental clinic ("Bright Smile Dental Clinic")

**Folder:** `task2-whatsapp-phase1/`
```
task2
│   ├── flow-diagram.mermaid       ← full conversation state  webhook-test-screenshot.png ← add your own screenshot here
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

**Architecture:** WhatsApp sandbox (Meta Cloud API, free developer tier) → `cloudflared` tunnel →
n8n Webhook node → Respond to Webhook node.

n8n's own static domain (from Task 1) is ngrok, but ngrok's free tier serves an interstitial page
(`ERR_NGROK_6024`) to any automated caller — including Meta's webhook verification request — since
it can't send the header needed to bypass it. So the actual Meta connection runs through a second
tunnel, `cloudflared`, added as a third service in `docker/docker-compose.yml` (quick-tunnel mode by
default; instructions in that file for upgrading to a stable named tunnel).

**Steps taken:**
- Webhook node built with `multipleMethods: true` (handles both Meta's `GET` verification handshake
  and real `POST` message deliveries on the same path).
- Respond to Webhook node returns `{{ $json.query["hub.challenge"] }}` as plain text — required
  since Meta's verification rejects anything else (e.g. JSON-wrapped).
- Full webhook URL (`https://<cloudflared-url>/webhook/<id>`) pasted into Meta App Dashboard →
  WhatsApp → Configuration → Webhook, with a self-chosen Verify Token.
- **✅ Verification passed** — confirmed via a successful `GET` execution in n8n returning the
  correct `hub.challenge` value.
- **⏳ In progress:** confirming an actual incoming message (`POST`) reaches n8n. Verification
  succeeding only proves the URL/token are correct — it does not by itself confirm the `messages`
  webhook field is subscribed, which is a separate checkbox in Meta's Configuration page. Currently
  debugging why a WhatsApp reply hasn't produced a new `POST` execution yet.
- `workflow.json` exported once a real message execution is confirmed (see `assets/workflow.json`
  and `assets/webhook-test-screenshot.png`).

**Security note:** No Meta access token or verify token is committed to this repo. Add your own in
n8n's credential store (Workflow → Credentials), never in code or `.env` files that get pushed.

**What's NOT built yet (by design — that's Phase 2):** real calendar integration, actual message
routing logic in n8n, and the AI/NLP layer for free-text understanding.

---

## GitHub Work

This section documents how the repo itself is managed, separately from the automation content.

**Repo name:** `devsynt-ai-internship-Khizer

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


