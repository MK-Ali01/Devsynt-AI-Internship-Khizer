# Project 2 · Phase 1 — LangChain & LangGraph Learning Map

A single static HTML/CSS page summarizing what I learned this week about LangChain and LangGraph — chains, prompt templates, output parsers, memory, tool calling, LangGraph's graph model (nodes/edges/state/conditional edges), and orchestrator agents.

## How to run it

No build step, no dependencies. Just open `index.html` in any browser (double-click it, or right-click → Open With → your browser). `style.css` sits next to it and is loaded automatically. An internet connection is only needed to load the Google Fonts used for headings/code — the page still works fine without one, it just falls back to system fonts.

## What I learned this week

Chains and agents both wire together the same handful of pieces (prompts, the model call, parsing, memory, tools) — the real difference is *when* the step order gets decided: at build time for a chain, at runtime for an agent. LangGraph made that click for me by making the graph explicit: nodes do the work, edges say what runs next, state is what actually gets passed around, and conditional edges are where the "agent" behavior — branching and looping — comes from. That also clarified why our upcoming project wants an orchestrator agent instead of one big agent trying to do everything: it's really several distinct jobs (research, writing, formatting), and an orchestrator gives that routing one clear, extensible home.

## Folder contents

```
project2-phase1/
├── index.html            — the learning showcase page
├── style.css              — styling for the page
├── README.md               — this file
└── screenshots/
    └── page-preview.png    — screenshot of index.html running in a browser
```
