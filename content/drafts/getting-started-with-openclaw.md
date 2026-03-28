# Getting Started with OpenClaw

**Type:** Tutorial | **Area:** OpenClaw | **Writer:** Writer 1
**Status:** 📋 Outline (pending editor review)
**Target date:** 2026-04-10

---

## Outline

### Introduction (2-3 sentences)
Set the hook: you have chat apps on every device, and you have AI assistants locked in browser tabs. OpenClaw connects them — a self-hosted gateway that lets you message an AI agent from WhatsApp, Telegram, Discord, or iMessage. This tutorial takes you from zero to a working setup in under 15 minutes.

### What You'll Build
By the end of this tutorial, you will have:
- A running OpenClaw Gateway on your machine
- A working chat session via the browser Control UI
- (Optional) A connected messaging channel (WhatsApp or Telegram)

### Prerequisites
- **Node.js 24** (recommended) or Node 22 LTS (22.16+)
- An API key from a supported AI provider (Anthropic, OpenAI, etc.)
- ~15 minutes
- (Optional) A WhatsApp or Telegram account for channel setup

### What Is OpenClaw?
Brief explainer (keep it tight — this isn't the architecture article):
- Self-hosted gateway that bridges chat apps to AI agents
- Runs a single Gateway process on your machine
- Supports multiple channels simultaneously (WhatsApp, Telegram, Discord, iMessage, and more)
- Agent-native: built for tool use, sessions, memory, and multi-agent routing
- Open source (MIT licensed)

Include the high-level architecture diagram:
```
Chat apps → Gateway → AI agent (Pi)
```

### Step 1: Install OpenClaw
- Installer script method (recommended — handles Node detection):
  - macOS/Linux: `curl -fsSL https://openclaw.ai/install.sh | bash`
  - Windows (PowerShell): `iwr -useb https://openclaw.ai/install.ps1 | iex`
- Alternative: `npm install -g openclaw@latest`
- Verify installation: `openclaw --version`
- Troubleshooting: what to do if `openclaw` not found (PATH issue)

### Step 2: Run the Onboarding Wizard
- Run `openclaw onboard --install-daemon`
- Walk through the wizard steps:
  1. **Model/Auth** — choose a provider, paste your API key, pick a default model
  2. **Workspace** — accept default or choose a custom location
  3. **Gateway** — port (default 18789), auth token (auto-generated)
  4. **Channels** — skip for now (or set one up — covered in Step 5)
  5. **Daemon** — installs as a background service
- QuickStart vs Advanced mode (recommend QuickStart for first-timers)

### Step 3: Verify the Gateway Is Running
- `openclaw gateway status` — confirm it's up
- `openclaw doctor` — check for any config issues
- What to do if it's not running: `openclaw gateway start`

### Step 4: Your First Chat
- Open the Control UI: `openclaw dashboard` (or navigate to `http://127.0.0.1:18789/`)
- Send a test message in the browser chat
- Explain what just happened: message → Gateway → AI agent → response → browser
- Screenshot placeholder: Control UI with a sample conversation

### Step 5: Connect a Messaging Channel (Optional)
Brief walkthrough for one or two popular channels:

#### WhatsApp
- Run `openclaw channels login` and scan the QR code
- Send a message from your phone — get a response
- Note: allowlist config for security (`channels.whatsapp.allowFrom`)

#### Telegram
- Create a bot via BotFather, get the token
- Add it to your config
- Send a message to the bot

Link to full channel docs for Discord, iMessage, Signal, etc.

### Step 6: Basic Configuration
- Config file location: `~/.openclaw/openclaw.json`
- Key settings to know about:
  - `channels.whatsapp.allowFrom` — restrict who can message
  - `messages.groupChat.mentionPatterns` — control group chat activation
  - Gateway auth token — keep it secure
- Mention `openclaw configure` for guided config changes

### What's Next
- **Connect more channels:** Link to Channels docs
- **Explore the architecture:** Tease the "OpenClaw Architecture" article (Sprint 2)
- **Set up multi-agent routing:** Link to multi-agent docs
- **Mobile nodes:** Pair your phone for camera, voice, and Canvas features
- **Join the community:** Discord link

### Summary
Recap what we built, reinforce that this is a self-hosted setup they fully control. Encourage experimentation.

---

## Sources
- OpenClaw docs: https://docs.openclaw.ai
- GitHub: https://github.com/openclaw/openclaw
- Local docs at `/opt/homebrew/lib/node_modules/openclaw/docs/`

## Notes for Editor
- Need to verify all CLI commands against current release before drafting
- Screenshots/diagrams needed: architecture diagram, Control UI, QR code flow
- The optional WhatsApp/Telegram section could be split into a separate article if this runs too long
- Style guide compliance: second person, present tense, college-level, code blocks with language specified
