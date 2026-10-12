# The recommended stack, and why

What Nathan actually runs, what each piece is for, and who can set it up. This file is public-safe: vendor names and reasoning only. **It's the source Nathan's agents answer `[kit-help] stack:` and `tools:` questions from.** If a question goes beyond it, they route it to Nathan rather than guess.

**The principle:** agents get their own accounts and a capped card. They never get your keys.

Who sets it up:
- 🤖 the agent does it
- 🤝 the agent drives and you click
- 🧑 you only: payments, banking, identity verification, OS-level approvals

## Your laptop agent (an extension of you)

| Tool | Why Nathan uses it | Needed | Who |
|---|---|---|---|
| **Claude Code** and/or **Codex** | The agent itself. Runs on the Claude Max or ChatGPT Pro subscription. Nathan runs both: Claude as the daily driver, and Codex as a second opinion and for computer-use-heavy work. | Core | 🧑 |
| **GitHub** | Getting and updating the kit. Version-controls your agent's instructions and skills. | Core | 🤝 |
| **Google Takeout** | Your full mail and calendar history, with no developer console or OAuth app. | Core | 🤝 |
| **Tailscale** | A private network, so your phone reaches the console and nothing is exposed to the public internet. It's free for personal use. | Recommended | 🤝 |
| **Beeper** | iMessage, WhatsApp, Signal and more in one place your agent can read. | Optional | 🤝 |
| **Fireflies** | Meeting transcripts flow into your knowledge base. It's the supported transcript source today. | Optional | 🤝 |
| **Ollama** | Meaning-based search that runs entirely on your Mac, with no API key and no data leaving. | Optional | 🤖 |
| **Telegram** | A reliable push channel for "agent needs you now" pings, like verification codes and distress calls. | Optional | 🤝 |
| **1Password** | Credentials in separate agent vaults with three tiers: *auto* (the agent may use it), *ask* (the agent requests it and you approve from your phone), and *never* (your banks, primary email and Apple ID, which the agent literally can't read). | Optional, later | 🤝 |

## An employee agent (later, optional: low access, high autonomy)

| Tool | Why | Needed | Who |
|---|---|---|---|
| **A spare Mac mini** | An always-on home for agents that work unattended, physically separate from your private data. It also takes heavy jobs like renders and bulk processing. | Optional | 🤝 |
| **Its own Gmail + GitHub** | The agent has its own identity and signs as "your assistant." The signature always matches whoever reviewed the message. | For employee | 🤝 (signup usually needs SMS) |
| **Mercury card** | A dedicated, capped card for the agents' own signups and small purchases. You see every charge, and the cap bounds the worst case. Every recurring charge gets logged with a cancel reminder. | For employee | 🧑 |
| **Cloudflare** | Free hosting, used only for things you choose to share publicly or with specific people. Everything else stays on your private network. | Optional | 🤝 |

## Common questions

- **Do I need API keys?** No for the core: your subscription runs it. Optional keys add things:
  - Gemini: full-history AI scoring
  - Voyage: better search ranking, with a free tier
  - OpenAI: live voice
- **Can I skip the Mac mini?** Yes. Everything starts on one Mac. Add the mini only when you want an agent that works on its own.
- **Why not one super-agent with all my access?** High access plus high autonomy means one malicious email can steer it. Keep the two kinds separate.
- **Something not listed here?** Ask, and if it's a judgment call, Nathan's agents will route it to him.
