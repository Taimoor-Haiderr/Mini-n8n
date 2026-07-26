# n8n Mini – Learn n8n Easily

A mobile-first, AI-powered learning companion for people who want to learn **n8n**, workflow automation, and AI agents without wading through documentation or long tutorials.

---

## Problem Statement

Beginners struggle to learn n8n because:

- Official documentation assumes prior automation knowledge.
- There are hundreds of nodes and no simple map of what to learn first.
- Workflows and JSON look intimidating before you understand the basics.
- Integrations (Gmail, WhatsApp, Slack, databases...) each have their own setup quirks.
- There's no beginner-friendly mentor to ask "why isn't this working?"

## Solution

n8n Mini bundles everything a beginner needs into one clean, dark-themed mobile app:

- A searchable library of **29 core nodes**, each explained in plain language (purpose, when to use it, operations, inputs/outputs, common mistakes, and a beginner tip).
- **12 integration guides** (Gmail, WhatsApp, Slack, OpenAI, Postgres, and more), each with a numbered, step-by-step "connect it" walkthrough.
- **14 complete workflow templates**, from a customer contact form to a restaurant WhatsApp bot and AI customer support.
- Bite-sized **lessons** covering every core concept (nodes, triggers, JSON, expressions, AI Agents, agentic AI, calling agents...).
- A **Learning Roadmap**, **Career Guide**, and **AI Business Ideas** section for people who want to freelance or build a business around n8n.
- An **AI Mentor chat assistant** — a floating chat button available on every screen — that answers open-ended questions in the same friendly, beginner-safe tone as the rest of the app.

## Features

- 📱 Mobile-first, responsive UI (works great on desktop too)
- 🔍 Global search across nodes, integrations, workflows, and lessons
- 🧭 Bottom tab navigation: Dashboard · Nodes · Integrations · Workflows · Learn · About
- 🤖 AI Mentor chat with typing indicator, quick prompts, and chat history
- 🎨 Dark, premium UI with glassmorphism, gradients, and micro-animations throughout
- 🗂 All educational content is static, local data — no backend or database required for the learning content itself

## AI Assistant

The AI Mentor is the app's main feature: a floating button on every screen opens a ChatGPT-style panel.

**System prompt** (see `src/data/systemPrompt.js`):

> You are the n8n Mentor, the friendly built-in AI assistant inside the "n8n Mini – Learn n8n Easily" app... Explain concepts in simple, plain language first... Prefer short paragraphs and bullet points... Give practical, concrete examples... Never invent specific product details... Stay strictly on topic: n8n, automation, AI agents/chatbots/calling agents, workflow design, integrations, and closely related career/business topics. If asked something unrelated, politely reply: "I specialize in helping users learn n8n, automation, AI agents, and workflow development."

It's wired to the **Groq API** (OpenAI-compatible, and free to start) using the `llama-3.3-70b-versatile` model. Swapping to **Gemini** or another OpenAI-compatible provider only requires editing `src/components/AIAssistant.jsx` — the fetch call, model name, and response parsing are all in one place.

> ⚠️ **Security note:** this demo calls the AI API directly from the browser using a `VITE_` env variable, which means the key is visible in the built client bundle. That's fine for local development and personal use. For a real production deployment, put the fetch call behind a small serverless function (Vercel/Netlify function, Cloudflare Worker, etc.) so your API key never reaches the browser.

## Tech Stack

- **React 18** + **Vite** — fast dev server and build
- **Custom CSS** (in `src/index.css`) for the full dark/glassmorphism theme and all animations
- **Tailwind CSS** wired in and ready to use for any new components (`tailwind.config.js` / `postcss.config.js` included)
- **lucide-react** for icons
- **Groq API** (or swap for Gemini/OpenAI) for the AI Mentor chat

## Project Structure

```
n8n-mini-learn/
├─ index.html
├─ package.json
├─ vite.config.js
├─ tailwind.config.js
├─ postcss.config.js
├─ .env.example
└─ src/
   ├─ main.jsx              # React entry point
   ├─ App.jsx                # Routing, nav, global search
   ├─ index.css              # Full theme, layout, and animations
   ├─ data/
   │  ├─ nodes.js            # 29 node definitions + category colors
   │  ├─ integrations.js     # 12 integration guides + setup steps
   │  ├─ workflows.js        # 14 workflow templates
   │  ├─ lessons.js          # Concept lessons
   │  ├─ content.js          # Roadmap, business ideas, career guide,
   │  │                      #   latest features, tips, mistakes, about, JSON guide
   │  └─ systemPrompt.js      # AI Mentor system prompt
   ├─ components/
   │  ├─ ui.jsx               # TopBar, Card, Pill, Section, DetailBlock, DetailList
   │  └─ AIAssistant.jsx      # Floating button + chat panel + Groq API call
   └─ pages/
      ├─ Dashboard.jsx
      ├─ Nodes.jsx            # NodesList + NodeDetail
      ├─ Integrations.jsx     # IntegrationsList + IntegrationDetail
      ├─ Workflows.jsx        # WorkflowsList + WorkflowDetail
      ├─ Learn.jsx            # Learn + LessonDetail
      ├─ Extras.jsx           # Roadmap, Business Ideas, Career, Latest Features
      └─ About.jsx
```

## Installation

```bash
# 1. Install dependencies
npm install

# 2. Add your AI key (optional — the app works fine without it,
#    the AI Mentor just shows a setup message instead of replying)
cp .env.example .env
# then edit .env and paste your key from https://console.groq.com/keys

# 3. Run the dev server
npm run dev
```

Open the URL Vite prints (usually `http://localhost:5173`).

## Build & Deployment

```bash
npm run build     # outputs a production build to /dist
npm run preview   # preview the production build locally
```

`/dist` is a static site — deploy it as-is to **Vercel**, **Netlify**, **Cloudflare Pages**, or any static host. Remember to set `VITE_GROQ_API_KEY` as an environment variable in your hosting provider's dashboard (not just your local `.env`) if you want the AI Mentor to work in production — and see the security note above about moving the API call server-side for a public deployment.

## Notes

- All learning content (nodes, integrations, workflows, lessons, roadmap, career guide, business ideas) is static local data — nothing here requires a database or backend.
- The AI Mentor is the only part of the app that makes a network call, and only when the person sends it a message.
