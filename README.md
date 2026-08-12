# Devsynt-AI-Internship-Khizer
This repo will contain all work done under Devsynt Intersnship such as:
Weekly Tasks, notes, pics, files, work updates.

# Task 1 — Set Up n8n and Push Progress to GitHub

**Date Submitted:** 29 July 2026

## Tech Stack
- n8n (workflow automation platform)
- Docker / Docker Compose
- ngrok (static domain tunnel)

## Tools
- Docker Desktop
- ngrok account + claimed static domain
- GitHub

## Nodes Used
N/A — this task is infrastructure setup only, no n8n workflow built yet.

## Files
- `docker/docker-compose.yml` — runs n8n + ngrok together
- `docker/.env.example` — template for required environment variables
- Submission screenshot — n8n editor open via static ngrok URL (not localhost)

# Task 2 — WhatsApp Lead-to-Booking Automation, Phase 1

**Date Submitted:** 29 July 2026 [DD Month YYYY]

## Tech Stack
- n8n (workflow automation)
- Meta WhatsApp Cloud API (sandbox)
- Docker / Docker Compose
- ngrok (browser access) + cloudflared (webhook tunnel)

## Tools
- Meta for Developers (WhatsApp product, sandbox test number)
- Cloudflare Tunnel (cloudflared, quick tunnel mode)
- mermaid.live (flow diagram)
- GitHub

## Nodes Used
1. Webhook (`multipleMethods: true` — handles GET verification + POST messages)
2. Respond to Webhook (returns `hub.challenge` as plain text for Meta verification)

## Files
- `assets/flow-diagram.mermaid` — full conversation state diagram (State 0–5, nudges, handoff)
- `messages.md` — all 9 required bot messages in English + Arabic
- `config.json` — reusable per-client config (clinic name, services, calendar ID, handoff contact)
- `workflow.json` — exported n8n workflow
- `assets/webhook-test-screenshot.png` — proof of successful webhook execution

#Project-1 SlotWise — AI Booking Concierge Bot

**Date Submitted:** 08 August 2026

## Tech Stack
- n8n (workflow automation)
- Discord Bot API
- Google Gemini API (intent/entity extraction)
- Google Sheets (booking + session storage)

## Tools
- Discord Developer Portal (bot application + token)
- Google AI Studio (Gemini API key)
- Google Sheets (Sessions / Bookings / Handoffs tabs)

## Nodes Used
1. Discord Trigger
2. Google Sheets — Lookup Rows (get session)
3. IF (session exists?)
4. Set + Google Sheets Append (init new session)
6. HTTP Request (Gemini — intent & entity extraction)
7. Code (parse LLM JSON response)
8. Set (build reply text per state)
10. Google Sheets — Append Row (log booking)
11. Google Sheets — Append Row (log handoff)
12. Discord — Send Message

## Files
- `workflow.json` — exported n8n workflow
- `workflow-screenshot.png` — visual canvas of connected nodes
